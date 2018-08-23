---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: MVC 5 vytvořte aplikaci pomocí služby Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5, která umožňuje uživatelům přihlášení pomocí OAuth 2.0 s přihlašovacími údaji z externí authenti...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 330cb290668ae951e822b95990ed92100b790cd5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752628"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Vytvoření aplikace ASP.NET MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování (C#)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5, která umožňuje uživatelům přihlášení pomocí [OAuth 2.0](http://oauth.net/2/) s přihlašovacími údaji z externí zprostředkovatel ověřování, jako je Facebook, Twitter, LinkedIn, Microsoft nebo Google. Pro zjednodušení tento kurz se zaměřuje na práci s přihlašovacích údajů z Facebooku nebo Googlu.
> 
> Povolení tyto přihlašovací údaje ve webových stránek poskytuje výraznou výhodu, protože milionům uživatelů již mají účty u těchto externích poskytovatelů. Tito uživatelé mohou být více ochotni zaregistrujte svůj web, pokud nemají k vytvoření a zapamatovat si novou sadu přihlašovacích údajů.
> 
> Viz také [aplikace ASP.NET MVC 5 s SMS a e-mailu Dvojúrovňového ověřování](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Kurz také ukazuje, jak přidat data profilu pro uživatele a jak používat rozhraní API členství k přidání rolí. V tomto kurzu zapsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (podle mě prosím na Twitteru: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Začínáme

Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalace sady Visual Studio [2013 s aktualizací 3](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší. Nápovědu k Dropboxu, Githubu, Linkedin, Instagramu, vyrovnávací paměti, Salesforce, datový proud, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! a další, najdete v tomto [ukázkový projekt](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Musíte nainstalovat Visual Studio [2013 s aktualizací 3](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší, k používání služby Google OAuth 2 a ladit lokálně bez upozornění protokolu SSL.


Klikněte na tlačítko **nový projekt** z **Start** stránky, nebo můžete použít v nabídce a vyberte **souboru**a potom **nový projekt**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Klikněte na tlačítko **nový projekt**a pak vyberte **Visual C#** na levé straně, pak **webové** a pak vyberte **webová aplikace ASP.NET**. Pojmenujte svůj projekt "MvcAuth" a potom klikněte na tlačítko **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

V **nový projekt ASP.NET** dialogového okna, klikněte na tlačítko **MVC**. Pokud není ověřování **jednotlivé uživatelské účty**, klikněte na tlačítko **změna ověřování** tlačítko a vyberte **jednotlivé uživatelské účty**. Zaškrtnutím **hostovat v cloudu**, aplikace bude velmi snadno hostovat v Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Pokud jste vybrali **hostovat v cloudu**, dokončete dialogové okno Konfigurovat.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Pomocí balíčku NuGet aktualizujte na nejnovější middleware OWIN

Aktualizovat pomocí Správce balíčků NuGet [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Vyberte **aktualizace** v levé nabídce. Můžete kliknout na **Aktualizovat vše** tlačítko nebo je můžete vyhledat pouze balíčky OWIN (viz následující obrázek):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Na následujícím obrázku jsou zobrazeny pouze OWIN balíčky:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Z balíčku správce konzoly (konzolu PMC), můžete zadat `Update-Package` příkaz, který bude aktualizovat všechny balíčky.

Stisknutím klávesy **F5** nebo **Ctrl + F5** ke spuštění aplikace. Na následujícím obrázku je číslo portu 1234. Při spuštění aplikace se zobrazí jiné číslo portu.

V závislosti na velikosti okna prohlížeče, možná budete muset klikněte na ikonu navigační zobrazíte **Domů**, **o**, **kontakt**, **zaregistrovat**a **přihlášení** odkazy.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Nastavení protokolu SSL v projektu

Pro připojení k zprostředkovatele ověřování, jako je Google nebo Facebook, musíte nastavit službu IIS Express pro použití protokolu SSL. Je důležité zachovat po přihlášení pomocí protokolu SSL a ne zase k protokolu HTTP, vaše přihlašovací soubor cookie je stejně jako tajný kód jako uživatelské jméno a heslo a bez použití protokolu SSL, které e-mail posíláte, ve formátu prostého textu sítí. Kromě toho jsme už používá čas k provedení signalizace a zabezpečený kanál (která je větší část kvůli tomu HTTPS pomalejší než HTTP) předtím, než kanál MVC se spustí, proto přesměrování zpět k protokolu HTTP, poté, co jste se přihlásili neprovede aktuální požadavek nebo budoucnost požadavky na mnohem rychlejší.

1. V **Průzkumníka řešení**, klikněte na tlačítko **MvcAuth** projektu.
2. Stisknutím klávesy F4 zobrazíte vlastnosti projektu. Alternativně z **zobrazení** nabídce vyberete **okno vlastností**.
3. Změna **protokol SSL povolený** na hodnotu True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Zkopírujte adresu URL protokolu SSL (který bude `https://localhost:44300/` Pokud jste vytvořili ostatní projekty SSL).
5. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **MvcAuth** projektu a vyberte **vlastnosti**.
6. Vyberte **webové** kartu a pak vložte adresu URL protokolu SSL do **adresa Url projektu** pole. Uložte soubor (seznam Ctl + S). Budete potřebovat tato adresa URL ke konfiguraci ověřování aplikace Facebook nebo Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Přidat [atribut RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atribut `Home` řadič tak, aby vyžadovala všechny požadavky musí používat protokol HTTPS. Více zabezpečený přístup, je přidat [atribut RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtr, aby se aplikace. V části &quot;chránit aplikace pomocí protokolu SSL a atribut Autorizovat&quot; v mé tutoral [vytvoření aplikace ASP.NET MVC pomocí ověření a SQL DB a nasazení do služby Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Část kontroler Home je uveden níže.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Stisknutím kláves CTRL + F5 spusťte aplikaci. Pokud jste nainstalovali certifikát v minulosti, můžete přeskočit zbývající část a přejděte do [vytvoření aplikace služby Google OAuth 2 a připojení aplikace k projektu](#goog), v opačném případě postupujte podle pokynů důvěřovat certifikát podepsaný svým držitelem certifikát, který vygenerovala služba IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Čtení **upozornění zabezpečení** dialogového okna a pak klikněte na tlačítko **Ano** Pokud chcete nainstalovat certifikát představující localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE ukazuje, *Domů* stránce a nebudou již existovat žádná upozornění protokolu SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome také certifikát přijme a zobrazí obsah HTTPS bez upozornění. Firefox používá své vlastní úložiště certifikátů, takže se zobrazí upozornění. Pro naši aplikaci můžete bez obav kliknout na **beru na vědomí rizika**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Vytvoření aplikace služby Google OAuth 2 a připojení aplikace k projektu

> [!WARNING]
> Aktuální Google OAuth pokyny najdete v tématu [ověřování Google konfigurace v ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Přejděte [konzole pro vývojáře Google](https://console.developers.google.com/).
2. Pokud jste ještě nevytvořili projektu před, vyberte **pověření** karta vlevo a pak vyberte **vytvořit**.
3. Na levé kartě klikněte **pověření**.
4. Klikněte na tlačítko **Vytvořte přihlašovací údaje** pak **ID klienta OAuth**. 

    1. V **vytvořit ID klienta** dialogového okna, ponechte výchozí **webovou aplikaci** pro typ aplikace.
    2. Nastavte **oprávnění JavaScript** počátky k adresa URL protokolu SSL, které jste použili výše (`https://localhost:44300/` Pokud jste vytvořili ostatní projekty SSL)
    3. Nastavte **identifikátor URI pro přesměrování autorizovaní** na:  
         `https://localhost:44300/signin-google`
5. Kliknutím na položku nabídky souhlas OAuth obrazovky a pak nastavte vaše e-mailovou adresu a název produktu. Po dokončení klikněte na formulář **Uložit**.
6. Kliknutím na položku nabídky knihovny, Hledat **rozhraní API Google +**, klikněte na něj a stiskněte povolit.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   Následující obrázek ukazuje povolených rozhraní API.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Ze služby Google API rozhraní API Správce, přejděte **pověření** kartu získat **ID klienta**. Stažení a uložení souboru JSON s tajných klíčů aplikací. Zkopírujte a vložte **ClientId** a **ClientSecret** do `UseGoogleAuthentication` metoda nalezena v *Startup.Auth.cs* ve *App_Start* složky. **ClientId** a **ClientSecret** hodnoty zobrazené níže jsou uvedeny ukázky a nefungují.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Zabezpečení – nikdy ukládání citlivých dat ve zdrojovém kódu. Účet a přihlašovací údaje se přidají do výše uvedený pro zjednodušení ukázkový kód. Zobrazit [osvědčené postupy pro nasazení hesel a dalších citlivých dat do ASP.NET a službě Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Stisknutím klávesy **CTRL + F5** sestavíte a spustíte aplikaci. Klikněte na tlačítko **přihlášení** odkaz.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. V části **přihlášení pomocí jiné služby**, klikněte na tlačítko **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Pokud přeskočíte, některé z výše uvedených kroků se zobrazí chyba HTTP 401. Znovu zkontrolujte vaše výše uvedených kroků. Pokud přeskočíte požadované nastavení (například **název produktu**), přidejte chybějící položky a uložte; může trvat několik minut, aby ověřování fungovalo.
10. Budete přesměrováni na web Google, kam zadáte svoje přihlašovací údaje.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Po zadání svých přihlašovacích údajů se výzva k udělení oprávnění k webové aplikaci, kterou jste právě vytvořili:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Klikněte na tlačítko **přijmout**. Budete teď přesměrováni zpět **zaregistrovat** stránky MvcAuth aplikace, ve kterém můžete zaregistrovat účet služby Google. Máte možnost změnit název registrace místní e-mailu použitý pro váš účet Gmailu, ale obecně je žádoucí uchovat výchozí e-mailový alias (to znamená, ten, který jste použili pro ověřování). Klikněte na tlačítko **zaregistrovat**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Vytvoření aplikace na Facebooku a připojení aplikace k projektu

> [!WARNING]
> Aktuální Facebook OAuth2 ověřování najdete v tématu [ověřování konfigurace služby Facebook](/aspnet/core/security/authentication/social/facebook-logins)


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Prozkoumejte Data členství

V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Rozbalte **objekt DefaultConnection (MvcAuth)**, rozbalte **tabulky**, klikněte pravým tlačítkem myši **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers tabulkových dat](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Přidání dat profilu do třídy uživatelů

V této části přidáte datum narození a domácí městě k uživatelským datům během registrace, jak je znázorněno na následujícím obrázku.

![REG s domácí města a Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Otevřít *Models\IdentityModels.cs* a přidejte narození data a Domovská stránka městě vlastnosti:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Otevřít *Models\AccountViewModels.cs* souboru a sada narození vlastnosti data a Domovská stránka městě v `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Otevřít *Controllers\AccountController.cs* a přidejte kód pro město Domovská stránka a datum narození v `ExternalLoginConfirmation` metody akce, jak je znázorněno:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Přidejte datum narození a domácí městě, jež *Views\Account\ExternalLoginConfirmation.cshtml* souboru:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Odstraňte databázi členství, aby znovu zaregistrovat Facebookový účet s vaší aplikací a ověřte, že můžete přidat nové datum narození a informace o profilu domácí města.

Z **Průzkumníka řešení**, klikněte na tlačítko **zobrazit všechny soubory** ikonu a pak klikněte pravým tlačítkem *přidat\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* a klikněte na tlačítko **odstranit**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet**, pak klikněte na tlačítko **Konzola správce balíčků** (PMC). Zadejte následující příkazy v konzole PMC.

1. Povolení migrace
2. Přidejte migraci Init
3. Aktualizace databáze

Spusťte aplikaci a používat k přihlášení a registraci někteří uživatelé Facebooku nebo Googlu.

## <a name="examine-the-membership-data"></a>Prozkoumejte Data členství

V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Klikněte pravým tlačítkem myši **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown` a `BirthDate` níže jsou zobrazena pole.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Odhlášení od vaší aplikace a přihlásíte se pomocí jiného účtu

Pokud přihlášení do vaší aplikace pomocí Facebooku a potom odhlaste se a zkuste se přihlásit znovu s jiným účtem Facebooku (pomocí stejného prohlížeče), můžete se ihned přihlášeni ke předchozí Facebookový účet, který jste použili. Chcete-li použít jiný účet, budete muset přejít na Facebook a odhlášení na Facebooku. Stejné pravidlo platí pro všechny ostatní 3. stran zprostředkovatele ověřování. Alternativně můžete protokolovat se pomocí jiného účtu s použitím jiného prohlížeče.

## <a name="next-steps"></a>Další kroky

V tématu [představení zprostředkovatele zabezpečení Yahoo a OAuth pro LinkedIn pro OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) podle Jerrie Pelser pokyny Yahoo a LinkedIn. Naleznete v tématu Jerrie to je dobré přihlášení prostřednictvím sociální sítě tlačítka pro ASP.NET MVC 5 se získat tlačítka povolit přihlášení prostřednictvím sociální sítě.

Využijte tento kurz [vytvoření aplikace ASP.NET MVC pomocí ověření a SQL DB a nasazení do služby Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), který pokračuje v tomto kurzu a zobrazuje následující:

1. Postup nasazení aplikace do Azure.
2. Jak zabezpečit aplikaci pomocí rolí.
3. Jak zabezpečit svou aplikaci pomocí [atribut RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) a [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtry.
4. Jak používat členské rozhraní API pro přidání uživatelů a rolí.

Jak vám v tomto kurzu líbilo a co můžeme zlepšit nám prosím zpětnou vazbu. Můžete také požádat o nový témat na [Show Me jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Dokonce je možné požádat o a hlasovat o nové funkce, které se přidají do technologie ASP.NET. Například můžete hlasovat pro nástroj pro [vytvoření a správě uživatelů a rolí.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Dobré vysvětlení fungování technologie ASP.NET externí ověřovací služby najdete v tématu Robert McMurray [externí ověřovací služby](https://asp.net/web-api/overview/security/external-authentication-services). Robert na článek taky obsahuje podrobnosti při povolení ověřování společnosti Microsoft a Twitter. Petr Dykstra je vynikající [kurz EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) ukazuje, jak pracovat s rozhraním Entity Framework.
