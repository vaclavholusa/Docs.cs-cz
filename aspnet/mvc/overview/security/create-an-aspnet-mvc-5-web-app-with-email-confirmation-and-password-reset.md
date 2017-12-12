---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "Vytvoření zabezpečeného webové aplikace ASP.NET MVC 5 se přihlásit, e-mailem potvrzení a heslo resetovat (C#) | Microsoft Docs"
author: Rick-Anderson
description: "Tento kurz ukazuje, jak sestavit webové aplikace ASP.NET MVC 5 s potvrzení e-mailu a resetování hesla pomocí systému členství ASP.NET Identity. Můžete certifikační autority..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: e3d8ad6e00b7fcb95f1c9bbe556021269c1a0624
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Vytvoření zabezpečeného webové aplikace ASP.NET MVC 5 se přihlásit, e-mailem potvrzení a heslo resetovat (C#)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Tento kurz ukazuje, jak sestavit webové aplikace ASP.NET MVC 5 s potvrzení e-mailu a resetování hesla pomocí systému členství ASP.NET Identity. Hotová aplikace si můžete stáhnout [zde](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Stahování obsahuje ladění pomocné rutiny, které umožňují testování potvrzení e-mailu a SMS bez nastavení e-mailem nebo poskytovatele služby SMS.
> 
> V tomto kurzu napsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Vytvoření aplikace ASP.NET MVC

Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší.

> [!NOTE]
> Upozornění: Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší k dokončení tohoto kurzu.


1. Vytvořte nový projekt ASP.NET Web a vyberte šablonu MVC. Webové formuláře také podporuje ASP.NET Identity, takže můžete sledovat v aplikaci web forms podobným způsobem.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**. Pokud chcete hostovat aplikaci v Azure, nechte políčko zaškrtnuto. Později v tomto kurzu jsme nasadit do Azure. Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).
3. Nastavte [projektu pro použití protokolu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Spuštění aplikace, klikněte na tlačítko **zaregistrovat** propojit a zaregistrovat uživatele. V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atribut.
5. V Průzkumníku serveru, přejděte na **Data Connections\DefaultConnection\Tables\AspNetUsers**, klikněte pravým tlačítkem a vyberte **otevřete definici tabulky**.

    Na následujícím obrázku `AspNetUsers` schématu:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Klikněte pravým tlačítkem na **AspNetUsers** tabulky a vyberte **zobrazit Data tabulky**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 V tomto okamžiku není potvrzena e-mailu.
7. Klikněte na řádek, a vyberte možnost odstranit. Budete v dalším kroku znovu přidat tuto e-mailu a odešlete e-mail s potvrzením.

## <a name="email-confirmation"></a>Potvrzení e-mailu

Je osvědčeným postupem potvrďte e-mailu nové registrace uživatele ověřit, že nejsou zosobnění někdo jiný (to znamená, že nebyly zaregistrována někoho jiného e-mailu). Předpokládejme, že jste měli diskusní fórum, chcete zabránit `"bob@example.com"` registraci jako `"joe@contoso.com"`. Bez potvrzení e-mailu `"joe@contoso.com"` může získat nežádoucí e-mailu z vaší aplikace. Předpokládejme, že Bob, ať už náhodně registrován jako `"bib@example.com"` a kdyby si všimli, si nebude moct používat heslo obnovit, protože aplikace nemá správnou e-mailovou. Potvrzení e-mailu poskytuje jen omezenou ochranu z robotů a neposkytuje ochranu proti určené odesílatelům nevyžádané pošty, mají mnoho aliasy pracovní e-mailu, které můžete použít k registraci.

Obvykle budete chtít zabránit noví uživatelé předtím, než byl potvrzen e-mailu, textovou zprávu SMS nebo jiný mechanismus publikování všechna data na webové stránky. <a id="build"></a>V následujících částech se budeme povolí potvrzení e-mailu a upravit kód nově zaregistrovaný uživatelům zabránit protokolování, dokud bylo potvrzeno e-mailu.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Propojte sendgrid vám umožňuje

I když tento kurz ukazuje jenom postup přidání e-mailové oznámení prostřednictvím [sendgrid vám umožňuje](http://sendgrid.com/), můžete odeslat e-mailu pomocí protokolu SMTP a další mechanismy (viz [další prostředky](#addRes)).

1. V konzole Správce balíčků, zadejte následující příkaz: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Přejděte na [sendgrid vám umožňuje Azure registrační stránku](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) a ZDARMA zaregistrovat účet sendgrid vám umožňuje. Přidejte kód podobná následující příkaz pro konfiguraci sendgrid vám umožňuje:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Budete muset přidat že následující obsahuje:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Pro zjednodušení této ukázku, uložíme nastavení aplikace v *web.config* souboru:

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu. Účet a přihlašovací údaje jsou uloženy v appSetting. V Azure, můžete bezpečně uložit tyto hodnoty na  **[konfigurace](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  na portálu Azure. V tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Povolit potvrzení e-mailu v kontroleru účtu

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Ověřte *Views\Account\ConfirmEmail.cshtml* soubor má syntaxi razor správné. (@ – Znak v prvním řádku chybět. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Spusťte aplikaci a klikněte na odkaz registrace. Po odeslání formuláře registrace jste přihlášení.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Zkontrolujte e-mailový účet a klikněte na odkaz k potvrzení e-mailu.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Požadovat potvrzení e-mailu před přihlášení

Aktuálně po dokončení registračním formuláři Uživatel se přihlásí. Chcete obecně potvrzení e-mailu před jejich protokolování. V následující části jsme se změnit kód tak, aby vyžadovala nové uživatele tak, aby měl potvrzen e-mailu, než se přihlásí (ověřený). Aktualizace `HttpPost Register` metoda s následující zvýrazněný změny:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Pomocí komentářů se `SignInAsync` metoda, nebude uživatel přihlášený pomocí registrace. `TempData["ViewBagLink"] = callbackUrl;` Řádku lze provádět [ladění aplikace](#dbg) a testování registrace bez odeslání e-mailu. `ViewBag.Message`slouží k zobrazení potvrdit pokynů. [Stažení ukázky](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) obsahuje kód pro testování potvrzení e-mailu bez nastavení e-mailu a můžete také použít k ladění aplikace.

Vytvoření `Views\Shared\Info.cshtml` souboru a přidejte následující kód razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Přidat [autorizovat atribut](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) k `Contact` metody akce kontroleru domovské. Můžete kliknutím na **kontaktujte** odkaz ověřte anonymní uživatelé nemají přístup k a ověření uživatelé mají přístup.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Také musíte aktualizovat `HttpPost Login` metodu akce:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Aktualizace *Views\Shared\Error.cshtml* zobrazení chybová zpráva:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Odstranit všechny účty v **AspNetUsers** tabulku, která obsahují e-mailový alias, který chcete otestovat s. Spusťte aplikaci a ověřte, že se nemůže přihlásit až se ujistíte, e-mailovou adresu. Jakmile potvrdíte, e-mailovou adresu, klikněte na **obraťte se na** odkaz.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Obnovení nebo resetování hesla

Odeberte komentář znaky z `HttpPost ForgotPassword` metody akce v kontroleru účet:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Odeberte komentář znaky z `ForgotPassword` [ActionLink](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) v *Views\Account\Login.cshtml* souboru nástroje razor zobrazení:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Přihlašovací stránky se nyní zobrazí odkaz k resetování hesla.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Odkaz pro potvrzení znovu odeslat e-mailu

Jakmile uživatel vytvoří nový místní účet, jsou e-mailem potvrzení propojení, které musí předtím, než se nemohou přihlásit Pokud uživatel, ať už náhodně odstraní e-mail s potvrzením nebo e-mailu nikdy dorazí, potřebují odkaz potvrzení odeslat znovu. Následující změny kódu ukazují, jak povolit tuto funkci.

Přidejte následující metodu helper k dolnímu okraji *Controllers\AccountController.cs* souboru:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Aktualizujte metodu registrace k použití nového pomocníka:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Aktualizujte metodu přihlášení k opětovnému odeslání hesla při Pokud nebyl potvrzen uživatelský účet:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Kombinování sociálních a místní přihlášení účtů

Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociálních účty. V tomto pořadí  **RickAndMSFT@gmail.com**  je poprvé vytvořen jako místní přihlášení, ale můžete vytvořit účet jako sociálních protokolu v prvním a pak přidejte místní přihlášení.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Klikněte na **spravovat** odkaz. Poznámka: externí 0 (sociální přihlášení) spojené s tímto účtem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Klikněte na odkaz na jiný protokol ve službě service a přijímat žádosti o aplikaci. Dva účty jsou spojena, nebudete moci přihlásit pomocí buď účtu. Můžete chtít uživatelům v případě, že jejich sociální přihlášení ověřovací služby, jsou vypnuty nebo jejich více pravděpodobně ztratili přístup k účtu mají sociálních přidat místní účty.

Na následujícím obrázku je tní sociálních přihlášení (což je vidět na **externích přihlášení: 1** zobrazený na stránce).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Kliknutím na **vyberte heslo** umožňuje přidat místního protokolu na související se stejným účtem.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Potvrzení e-mailu do větší hloubky

Moje kurzu [potvrzení účtu a heslo pro obnovení s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) přejde do tohoto tématu s dalšími podrobnostmi.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Ladění aplikace

Pokud neobdržíte e-mail, který obsahuje odkaz:

- Zkontrolujte složku Nevyžádaná pošta nebo nevyžádané pošty.
- Přihlaste se k účtu sendgrid vám umožňuje a klikněte na [odkaz e-mailu aktivity](https://sendgrid.com/logs/index).

Chcete-li otestovat ověřovací odkaz bez e-mailu, stáhněte [hotová ukázka](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Odkaz pro potvrzení a kódy potvrzení zobrazí na stránce.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Odkazy na identitě ASP.NET Identity doporučené prostředky](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Účet potvrzení a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) přejde do další podrobnosti o potvrzení hesla obnovení a účet.
- [Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) tohoto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s ověřování sítě Facebook a Google OAuth 2. Také ukazuje, jak přidat další data do databáze Identity.
- [Nasazení aplikace zabezpečené rozhraní ASP.NET MVC s členství, OAuth a databáze SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). V tomto kurzu přidá nasazení Azure, jak zabezpečit vaše aplikace s rolemi, jak používat členské rozhraní API k přidání uživatelů a rolí a další funkce zabezpečení.
- [Vytvoření aplikace na Google OAuth 2 a připojení aplikace k projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Vytvoření aplikace v síti Facebook a připojení aplikace k projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Nastavení protokolu SSL v projektu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
