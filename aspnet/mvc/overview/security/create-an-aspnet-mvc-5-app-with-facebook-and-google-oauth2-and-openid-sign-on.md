---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Vytvoření MVC 5 aplikace pomocí služby Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování (C#) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5, který umožňuje uživatelům přihlásit pomocí OAuth 2.0 přihlašovací údaje z externí authenti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: c289c209b50f0c2c1f2d8b15a3aedeaebf671d0b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Vytvoření aplikace ASP.NET MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování (C#)
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5, který umožňuje uživatelům přihlásit se pomocí [OAuth 2.0](http://oauth.net/2/) s přihlašovací údaje z externí zprostředkovatel ověřování, jako je Facebook, Twitter, LinkedIn, Microsoft nebo Google. Pro zjednodušení tento kurz se zaměřuje na práci s přihlašovacími údaji ze sítě Facebook a Google.
> 
> Povolení tyto přihlašovací údaje ve své weby nabízí významné výhody, protože milionům uživatelů již mají účty s tyto externí zprostředkovatele. Tyto uživatele může být více nakloněné zaregistrovat pro svůj web, pokud nemají k vytvoření a zapamatovat si novou sadu přihlašovacích údajů.
> 
> Viz také [aplikaci ASP.NET MVC 5 s SMS a e-mailu dvoufaktorové ověřování](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Tento kurz také ukazuje, jak přidat data profilu pro uživatele a jak používat rozhraní API členství a přidejte role. V tomto kurzu napsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (postupujte mi na Twitteru: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Začínáme

Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalaci sady Visual Studio [2013 s aktualizací 3](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší. Pomoc s Dropbox, Githubu, Linkedin, Instagram, vyrovnávací paměť, Salesforce, datový proud, zásobník Exchange, Tripit, Twitch, Twitter, Yahoo! a další najdete v tématu to [ukázkový projekt](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Je nutné nainstalovat Visual Studio [2013 s aktualizací 3](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší pro použijte Google OAuth 2 a ladění místně bez upozornění týkající se SSL.


Klikněte na tlačítko **nový projekt** z **spustit** stránky, nebo můžete použít nabídku a vyberte **soubor**a potom **nový projekt**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Klikněte na tlačítko **nový projekt**, pak vyberte **Visual C#** na levé straně, pak **webové** a pak vyberte **webové aplikace ASP.NET**. Název projektu "MvcAuth" a pak klikněte na **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

V **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **MVC**. Pokud ověření není **jednotlivé uživatelské účty**, klikněte **změna ověřování** tlačítko a vyberte **jednotlivé uživatelské účty**. Kontrolou **hostitel v cloudu**, aplikace bude velmi snadné hostovat v Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Pokud jste vybrali **hostitel v cloudu**, dokončete dialogové okno Konfigurovat.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Použít aktualizace na nejnovější middlewaru OWIN, který NuGet

Pomocí Správce balíčků NuGet se aktualizovat [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Vyberte **aktualizace** v levé nabídce. Kliknutím na **Aktualizovat vše** tlačítko, nebo můžete vyhledat pouze balíčky OWIN (viz další obrázek):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Na obrázku níže jsou uvedeny pouze OWIN balíčky:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Z balíček správce konzoly (pomocí PMC), můžete zadat `Update-Package` příkaz, který aktualizuje všechny balíčky.

Stiskněte klávesu **F5** nebo **Ctrl + F5** ke spuštění aplikace. Na obrázku níže číslo portu je 1234. Při spuštění aplikace se zobrazí jiné číslo portu.

V závislosti na velikosti okna prohlížeče, možná budete muset klikněte na ikonu navigace zobrazíte **Domů**, **o**, **kontaktujte**, **zaregistrovat**a **přihlásit** odkazy.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Nastavení protokolu SSL v projektu

Pro připojení k zprostředkovatele ověřování, jako je Google a Facebook, musíte nastavit službu IIS Express pro použití protokolu SSL. Je důležité udržovat po přihlášení pomocí protokolu SSL a není vyřadit zpět k protokolu HTTP, vaše přihlašovací soubor cookie je stejně jako tajný klíč jako uživatelské jméno a heslo a bez použití protokolu SSL odesíláte ho v prostém textu v drátové síti. Kromě toho jste už udělali čas k provedení handshake a zabezpečený kanál (což je velkou část díky HTTPS pomalejší než HTTP) před spuštěním kanál MVC, proto přesměrovat zpět na HTTP po jste přihlášeni nebude nastavit aktuální požadavek nebo budoucí požadavky na mnohem rychlejší.

1. V **Průzkumníku řešení**, klikněte **MvcAuth** projektu.
2. Stisknutím klávesy F4 zobrazit vlastnosti projektu. Můžete také z **zobrazení** nabídky můžete vybrat **vlastnosti – okno**.
3. Změna **SSL povoleno** na hodnotu True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Zkopírujte adresu URL protokolu SSL (který bude `https://localhost:44300/` Pokud jste vytvořili další projekty SSL).
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem **MvcAuth** projektu a vyberte **vlastnosti**.
6. Vyberte **webové** kartě a vložte adresu URL SSL do **adresa Url projektu** pole. Uložte soubor (seznam Ctl + S). Budete potřebovat adresu URL konfigurace aplikací pro ověřování sítě Facebook a Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Přidat [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atribut `Home` řadiče tak, aby vyžadovala všechny požadavky musí používat protokol HTTPS. Zabezpečení způsobů je přidání [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtru do aplikace. Najdete v části &quot;chránit aplikace pomocí protokolu SSL a atribut Autorizovat&quot; v mé tutoral [vytvoření aplikace ASP.NET MVC pomocí ověřování a databázi SQL a nasazení do Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Část domovské řadiče jsou uvedeny níže.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci. Pokud jste nainstalovali certifikát v minulosti, můžete přeskočit zbývající část tohoto oddílu a přejít na [vytvoření aplikace na Google OAuth 2 a připojení aplikace k projektu](#goog), jinak postupujte podle pokynů důvěřovat podepsaný certifikát, který služba IIS Express vygenerovala.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Pro čtení **upozornění zabezpečení** dialogové okno a potom klikněte na **Ano** Pokud chcete nainstalovat certifikát představující localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE ukazuje *Domů* stránky a neexistují žádná upozornění týkající se SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome také certifikát přijme a zobrazí obsah protokolu HTTPS bez upozornění. Firefox používá vlastní úložiště certifikátů, proto se zobrazí upozornění. Pro naši aplikaci můžete bezpečně kliknout na **beru na vědomí rizika**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Vytvoření aplikace na Google OAuth 2 a připojení aplikace k projektu

> [!WARNING]
> Aktuální Google OAuth pokyny najdete v tématu [ověřování Google konfigurace v ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Přejděte na [konzole pro vývojáře Google](https://console.developers.google.com/).
2. Pokud jste dosud nevytvořili projektu před, vyberte **pověření** v levé kartě a potom vyberte **vytvořit**.
3. V levé kartě klikněte na **pověření**.
4. Klikněte na tlačítko **vytvořit přihlašovací údaje** pak **ID klienta OAuth**. 

    1. V **vytvořit ID klienta** dialogové okno, ponechte výchozí **webové aplikace** pro typ aplikace.
    2. Nastavte **oprávnění JavaScript** zdroje na adresu URL SSL, který jste použili výše (`https://localhost:44300/` Pokud jste vytvořili další projekty SSL)
    3. Nastavte **identifikátor URI pro přesměrování autorizováno** na:  
         `https://localhost:44300/signin-google`
5. Klikněte na položku nabídky obrazovky souhlas OAuth a potom nastavit vaše e-mailovou adresu a název produktu. Když jste dokončili a klikněte na formulář **Uložit**.
6. Klikněte na položku nabídky knihovny, hledání **rozhraní API Google +**, klikněte na něm potom stiskněte klávesu povolit.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   Následující obrázek ukazuje povoleno rozhraní API.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Z Google rozhraní API rozhraní API Správce, přejděte **pověření** kartě získat **ID klienta**. Stáhněte si uložte soubor JSON s tajné klíče aplikace. Zkopírujte a vložte **ClientId** a **ClientSecret** do `UseGoogleAuthentication` nalezena metoda v *Startup.Auth.cs* v soubor *App_Start* složky. **ClientId** a **ClientSecret** hodnoty zobrazené níže jsou příklady a nefungují.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu. Účet a přihlašovací údaje se přidají do výše pro zjednodušení v ukázkovém kódu. V tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a službě Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Stiskněte klávesu **CTRL + F5** sestavení a spuštění aplikace. Klikněte **přihlásit** odkaz.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. V části **přihlásit pomocí jiné služby**, klikněte na tlačítko **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > V případě, že byste zapomněli na některý z výše uvedených kroků zobrazí se chyba HTTP 401. Znovu zkontrolujte vaše kroky uvedené výše. Pokud přeskočíte požadované nastavení (například **název produktu**), přidejte položku chybějící a uložte; může trvat několik minut, než ověřování pracovat.
10. Budete přesměrováni na web Google, kde bude zadejte svoje přihlašovací údaje.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Po zadání přihlašovacích údajů, zobrazí se výzva k udělit oprávnění k webové aplikaci, kterou jste právě vytvořili:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Klikněte na tlačítko **přijmout**. Budete teď přesměrováni zpět **zaregistrovat** stránka MvcAuth aplikace, kde můžete zaregistrovat účet služby Google. Máte možnost změny názvu registrace místní e-mailu, který se používá pro váš účet z Gmailu, ale chcete obecně zachovat výchozí e-mailový alias (to znamená, ten, který jste použili pro ověřování). Klikněte na tlačítko **zaregistrovat**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Vytvoření aplikace v síti Facebook a připojení aplikace k projektu

> [!WARNING]
> Aktuální pokyny ověřování Facebook OAuth2 najdete v tématu [Facebook konfigurace ověřování](/aspnet/core/security/authentication/social/facebook-logins)

Pro ověřování sítě Facebook OAuth2 musíte zkopírovat do projektu některá nastavení z aplikace, která vytvoříte ve službě Facebook.

1. V prohlížeči přejděte na [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) a přihlaste se zadáním přihlašovacích údajů služby Facebook.
2. Pokud nejsou už registrovaný jako vývojář Facebook, klikněte na tlačítko **zaregistrujte se jako vývojář** a postupujte podle pokynů k registraci.
3. Na **aplikace** , klikněte na **vytvořit novou aplikaci**.

    ![Vytvoření nové aplikace](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Zadejte **název aplikace** a **kategorie**, pak klikněte na tlačítko **vytvořit aplikaci**.

    Toto musí být jedinečný mezi Facebook. <strong>Aplikace Namespace</strong> je součástí adresu URL, kterou vaše aplikace bude používat pro přístup k aplikaci Facebook pro ověření (například https://apps.facebook.com/{App Namespace}). Pokud neurčíte <strong>aplikace Namespace</strong>, <strong>ID aplikace</strong> se použije pro adresu URL. <strong>ID aplikace</strong> je číslo dlouho generované systémem, který se zobrazí v dalším kroku.

    ![Vytvořit novou aplikaci dialogové okno](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Odešlete kontrola zabezpečení Standardní.

    ![Kontrola zabezpečení](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Vyberte **nastavení** pro pruhu levé nabídce![ Panel nabídek Facebook pro vývojáře](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. Na **základní** vyberte nastavení části stránky **přidejte platformu** k určení, že přidáváte webovou aplikaci. ![Základní nastavení](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Vyberte **webu** z možností platformu.  
  
    ![Volby platformy](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Poznamenejte si vaše **ID aplikace** a **tajný klíč aplikace** tak, aby později v tomto kurzu můžete přidat i do aplikace MVC. Také přidat adresu URL vašeho webu (`https://localhost:44300/`) k testování aplikace MVC. Také přidat **e-mailu kontaktujte**. Pak vyberte **uložit změny**.   

    ![Stránka Podrobnosti základní aplikace](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Všimněte si, že pouze bude možné provést ověření pomocí e-mailový alias, který je zaregistrovaný. Ostatní uživatele a testovací účty, nebude možné k registraci. Můžete udělit další Facebook účty přístup k aplikaci na k síti Facebook **vývojáře role** kartě.
10. V sadě Visual Studio otevřete *aplikace\_Start\Startup.Auth.cs*.
11. Zkopírujte a vložte **AppId** a **tajný klíč aplikace** do `UseFacebookAuthentication` metoda. **AppId** a **tajný klíč aplikace** hodnoty zobrazené níže jsou příklady a nebude fungovat.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Klikněte na tlačítko **uložit změny**.
13. Stiskněte klávesu **CTRL + F5** ke spuštění aplikace.


Vyberte **přihlásit** zobrazíte stránku přihlášení. Klikněte na tlačítko **Facebook** pod **přihlásit pomocí jiné služby.**

Zadejte přihlašovací údaje služby Facebook.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Zobrazí se výzva k udělení oprávnění pro přístup k vašim veřejný profil a seznamu friend aplikace.

![Podrobnosti o aplikaci Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Jste nyní přihlášeni. Nyní můžete zaregistrovat tento účet s aplikací.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Při registraci, bude položka přidána do *uživatelé* tabulky databáze členství.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Zkontrolujte Data členství

V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Rozbalte položku **objekt DefaultConnection (MvcAuth)**, rozbalte položku **tabulky**, klikněte pravým tlačítkem na **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![data tabulky aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Přidat Data profilu pro třídu uživatele

V této části přidáte datum narození a domácí městě k datům uživatele během registrace, jak je znázorněno na následujícím obrázku.

![REG s domácí města a Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Otevřete *Models\IdentityModels.cs* souboru a přidejte narození datum a z domova městě vlastnosti:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Otevřete *Models\AccountViewModels.cs* souboru a sada narození datum a z domova městě vlastnosti v `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Otevřete *Controllers\AccountController.cs* souboru a přidejte kód pro narození datum a z domova městě ve `ExternalLoginConfirmation` metody akce, jak je znázorněno:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Přidat datum narození a domácí městě, jež *Views\Account\ExternalLoginConfirmation.cshtml* souboru:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Abyste mohli znovu zaregistrovat svůj účet Facebook s vaší aplikací a ověřte, že můžete přidat nové datum narození a informace o profilu domácí města, odstraňte tuto databázi členství.

Z **Průzkumníku řešení**, klikněte **zobrazit všechny soubory** ikonu a potom klikněte pravým tlačítkem *přidat\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* a klikněte na tlačítko **odstranit**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Z **nástroje** nabídky, klikněte na tlačítko **Manager balíček NuGet**, pak klikněte na tlačítko **Konzola správce balíčků** (pomocí PMC). Zadejte následující příkazy v pomocí PMC.

1. Příkaz enable-Migrations
2. Init – přidat migrace
3. Update-Database

Spusťte aplikaci a používat sítě FaceBook a Google přihlášení a registrace někteří uživatelé.

## <a name="examine-the-membership-data"></a>Zkontrolujte Data členství

V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Klikněte pravým tlačítkem na **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown` a `BirthDate` pole, které jsou uvedeny níže.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Odhlášení vaší aplikace a přihlášením pomocí jiného účtu

Přihlaste se k vaší aplikace pomocí služby Facebook,,, potom se přihlaste a pokuste se přihlásit znovu s jiným účtem Facebook (pomocí stejného prohlížeče), budete okamžitě přihlášeni předchozí, které jste použili účet služby Facebook. Chcete-li použít jiný účet, budete muset přejděte do sítě Facebook a odhlaste v síti Facebook. Stejné pravidlo se vztahuje na všechny ostatní 3. stran zprostředkovatele ověřování. Alternativně můžete protokolovat se pomocí jiného účtu pomocí jiné prohlížeče.

## <a name="next-steps"></a>Další kroky

V tématu [představení zprostředkovatele zabezpečení Yahoo a OAuth pro LinkedIn pro OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) podle Jerrie Pelser pokyny Yahoo a LinkedIn. Najdete v článku na Jerrie velmi přihlášení prostřednictvím sociální sítě tlačítka pro ASP.NET MVC 5 se získat povolení přihlášení prostřednictvím sociální sítě tlačítka.

Využijte tento kurz [vytvoření aplikace ASP.NET MVC pomocí ověřování a databázi SQL a nasazení do Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), který pokračuje v tomto kurzu a zobrazuje následující:

1. Postup nasazení aplikace do Azure.
2. Jak zabezpečit aplikace s rolemi.
3. Jak zabezpečit vaší aplikace pomocí [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) a [Autorizovat](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtry.
4. Jak používat členské rozhraní API pro přidání uživatelů a rolí.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Dokonce můžete požádat o a hlasovat o nové funkce, které mají být přidány do technologie ASP.NET. Například můžete hlasovat pro nástroj, který [vytvoření a správě uživatelů a rolí.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Dobrý vysvětlení o fungování technologie ASP.NET externí ověřovací služby najdete v tématu Roberta Mcmurrayho [externí ověřovací služby](https://asp.net/web-api/overview/security/external-authentication-services). Robert je článek také obsahuje podrobnosti v povolení ověřování Microsoft a Twitter. Tní Dykstra je vynikající [kurz EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) ukazuje, jak pracovat s rozhraní Entity Framework.
