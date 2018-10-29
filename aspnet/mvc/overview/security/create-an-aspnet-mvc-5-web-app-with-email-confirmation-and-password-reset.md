---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Vytvoření zabezpečené webové aplikace ASP.NET MVC 5 s přihlášením, e-mailu potvrzení a resetováním hesla (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5 s e-mailové potvrzení a resetování hesla pomocí systém členství technologie ASP.NET Identity. Můžete certifikační autority...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 5092476c6cf59bea6fab6fa6f169ff11ec4c9c4a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207482"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Vytvoření zabezpečené webové aplikace ASP.NET MVC 5 s přihlášením, e-mailu potvrzení a resetováním hesla (C#)
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5 s e-mailové potvrzení a resetování hesla pomocí systém členství technologie ASP.NET Identity. Můžete stáhnout hotovou aplikaci [tady](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Položka ke stažení obsahuje ladění pomocné rutiny, které umožňují otestovat bez nastavení e-mailem nebo poskytovatele služby SMS serveru SMS a e-mailové potvrzení.
> 
> V tomto kurzu zapsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Vytvoření aplikace ASP.NET MVC

Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší.

> [!NOTE]
> Upozornění: Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší, k dokončení tohoto kurzu.


1. Vytvoření nového projektu ASP.NET Web a vyberte šablonu MVC. Webové formuláře také podporuje ASP.NET Identity, takže může podle podobných kroků ve webové aplikaci formulářů.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**. Pokud chcete hostovat aplikace ve službě Azure, ponechte políčko zaškrtnuté. Později v tomto kurzu nasadíme do Azure. Je možné [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Nastavte [projektu pro použití protokolu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Spuštění aplikace, klikněte na tlačítko **zaregistrovat** propojit a zaregistrovat uživatele. V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atribut.
5. V Průzkumníku serveru přejděte do **Data Connections\DefaultConnection\Tables\AspNetUsers**, klikněte pravým tlačítkem myši a vyberte **Otevřít definici tabulky**.

    Na následujícím obrázku `AspNetUsers` schématu:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Klikněte pravým tlačítkem na **AspNetUsers** tabulce a vybrat **zobrazit Data tabulky**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 V tomto okamžiku není potvrzena e-mailu.
7. Klikněte na řádek a vyberte odstranit. Budete znovu přidat tuto e-mailu v dalším kroku a odeslat e-mail s potvrzením.

## <a name="email-confirmation"></a>Potvrzení e-mailu

Je osvědčeným postupem je potvrďte e-mailu nové registrace uživatele k ověření, nejsou někdo zosobnění (to znamená, že se ještě nezaregistrovali někoho jiného e-mailu). Předpokládejme, že jste měli diskusní fórum, chcete zabránit `"bob@example.com"` registroval jako `"joe@contoso.com"`. Bez potvrzení e-mailu `"joe@contoso.com"` může získat nežádoucí e-mailu vaší aplikace. Předpokládejme, že Bob neúmyslně zaregistrovaný jako `"bib@example.com"` a kdyby si všimli, že nebudou moci používat obnovit heslo, protože aplikace nemá správnou e-mailovou. Potvrzení e-mailu zajišťuje pouze omezenou ochranu před roboty a neposkytuje ochranu z určené spammery, mají mnoho pracovní e-mailu aliasů můžete použít k registraci.

Obvykle chcete novým uživatelům zabránit v účtování žádná data k webu předtím, než byly potvrzeny e-mailem, textovou zprávu SMS nebo jiný mechanismus. <a id="build"></a>V následujících částech budeme povolit e-mailové potvrzení a upravovat kód, který nově zaregistrovaný uživatelům zabránit v přihlašování až do svého e-mailu byl potvrzen.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Připojení SendGrid

Pokyny v této části nejsou aktuální. Zobrazit [poskytovatele e-mailu Sendgridu konfigurace](/aspnet/core/security/authentication/accconfirm#configure-email-provider) pro aktualizace pokynů.

Tento kurz vysvětluje pouze přidání e-mailové oznámení prostřednictvím [SendGrid](http://sendgrid.com/), můžete poslat e-mailu pomocí protokolu SMTP a další mechanismy (naleznete v tématu [další prostředky](#addRes)).

1. V konzole Správce balíčků zadejte následující příkaz: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Přejděte [Azure SendGrid registrační stránku](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) a zaregistrujte si bezplatný účet SendGrid. Konfigurace služby SendGrid přidáním podobně jako v následujícím kódu *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Bude potřeba přidat následující zahrnuje:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Pro zjednodušení této ukázku, uložíme nastavení aplikace v *web.config* souboru:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Zabezpečení – nikdy ukládání citlivých dat ve zdrojovém kódu. Účet a přihlašovací údaje jsou uložené v nastavení appSetting. V Azure, můžete bezpečně uložit tyto hodnoty na **[konfigurovat](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** karta na portálu Azure portal. Zobrazit [osvědčené postupy pro nasazení hesel a dalších citlivých dat do ASP.NET a Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Povolení e-mailové potvrzení v kontroleru účtu

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Ověřte, *Views\Account\ConfirmEmail.cshtml* soubor nemá správnou syntaxí. (@ Znaků v prvním řádku můžou chybět. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Spusťte aplikaci a klikněte na odkaz zaregistrovat. Jakmile odešlete registrační formulář jste přihlášeni.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Zkontrolujte e-mailový účet a klikněte na odkaz pro potvrzení e-mailu.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Vyžadovat e-mailové potvrzení před přihlášení

Aktuálně po dokončení registrační formulář uživatele se přihlásí. Chcete obecně potvrzení jejich e-mailu před jejich přihlášení. V níže uvedené části upravíme kód tak, aby vyžadovala nové uživatele, aby potvrzeno e-mailu předtím, než se přihlásí (ověřit). Aktualizace `HttpPost Register` metodu s následující zvýrazněný změny:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Tak `SignInAsync` metoda, nebude uživatel přihlášený pomocí registrace. `TempData["ViewBagLink"] = callbackUrl;` Řádku je možné použít k [ladit aplikaci](#dbg) a testování registrace bez odeslání e-mailu. `ViewBag.Message` slouží k zobrazení pokynů potvrzení. [Stáhněte si ukázky](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) obsahuje kód pro testování potvrzení e-mailu bez nastavení e-mailu a můžete také použít k ladění aplikace.

Vytvoření `Views\Shared\Info.cshtml` a přidejte následující kód razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Přidat [Authorize atribut](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) k `Contact` metody akce kontroleru Domovská stránka. Můžete kliknout na **kontakt** odkaz pro ověření anonymní uživatelé nemají přístup a ověření uživatelé mají přístup.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Také musíte aktualizovat `HttpPost Login` metody akce:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Aktualizace *Views\Shared\Error.cshtml* zobrazení chybová zpráva:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Odstranit některé účty **AspNetUsers** tabulce, která obsahuje e-mailový alias, který chcete otestovat s. Spusťte aplikaci a ověřte, že se nemůže přihlásit až se ujistíte e-mailovou adresu. Jakmile potvrdíte, e-mailovou adresu, klikněte na tlačítko **kontakt** odkaz.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Obnovení/obnovení hesla

Odebere znaky komentáře z `HttpPost ForgotPassword` metody akce v kontroleru účtu:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Odebere znaky komentáře z `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) v *Views\Account\Login.cshtml* soubor zobrazení razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Přihlašovací stránky se nyní zobrazí odkaz resetovat heslo.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Znovu poslat potvrzovací odkaz e-mailu

Jakmile uživatel vytvoří nový místní účet, jsou e-mailem odkaz pro potvrzení, které je potřeba použít dřív, než se můžou přihlašovat Pokud uživatel omylem odstraní potvrzovací e-mail nebo nikdy přijde e-mailu, potřebují potvrzovací odkaz odeslán znovu. Následující změny kódu ukazují, jak ji povolit.

Přidejte následující pomocnou metodu k dolnímu okraji *Controllers\AccountController.cs* souboru:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Aktualizujte metodu registrace pomocí nového pomocníka:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Aktualizujte metodu přihlášení se opětovné poslání heslo, pokud nebyla potvrzena uživatelský účet:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Sloučit účty sociálních sítí a místní přihlášení

Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociální účty. V následujícím pořadí **RickAndMSFT@gmail.com** se nejprve vytvoří jako místní přihlášení, ale můžete vytvořit účet jako sociální protokolu v prvním a pak přidat místní přihlašovací údaje.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Klikněte na **spravovat** odkaz. Poznámka: **externí přihlášení: 0** spojený s tímto účtem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Klikněte na odkaz na jiný protokol služby a přijímání požadavků aplikace. Byli sloučeni dva účty, budou moct přihlásit pomocí obou. Můžete chtít uživatelům přidat místní účty v případě jejich sociálních sítí protokolu ve službě ověřování je mimo provoz nebo spíše se ztratili přístup k jejich účtu na sociální síti.

Na následujícím obrázku je Petr sociální přihlášení (které si můžete všimnout **externí přihlášení: 1** zobrazený na stránce).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Kliknutím na **výběru hesla** umožňuje přidat místního protokolu na spojené se stejným účtem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Potvrzení e-mailu do větší hloubky

Tento kurz [potvrzení účtu a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) přejde do tohoto tématu, s dalšími podrobnostmi.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Ladění aplikace

Pokud se vám e-mail s odkazem:

- Zkontrolujte složku s nevyžádanou nebo nevyžádané pošty.
- Přihlaste se do účtu SendGrid a klikněte na [odkaz e-mailu aktivita](https://sendgrid.com/logs/index).

Chcete-li otestovat odkaz pro ověření bez e-mailu, stáhněte si [úplnou vzorovou](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Potvrzovací odkaz potvrzení kódy se zobrazí na stránce.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Odkazy na ASP.NET Identity doporučené zdroje informací](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Potvrzení účtu a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) obsahuje větší podrobnosti o potvrzení hesla obnovení a účtu.
- [Aplikace MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) v tomto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s autorizací Facebook nebo Google OAuth 2. Také ukazuje, jak přidat další data do databáze nástroje Identity.
- [Nasaďte zabezpečené aplikace ASP.NET MVC pomocí členství, OAuth a SQL Database do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). V tomto kurzu přidá nasazení v Azure, jak zabezpečit vaši aplikaci s rolemi, jak můžete členské rozhraní API přidávat uživatele a role a další funkce zabezpečení.
- [Vytvoření aplikace služby Google OAuth 2 a připojení aplikace k projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Vytvoření aplikace na Facebooku a připojení aplikace k projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Nastavení protokolu SSL v projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
