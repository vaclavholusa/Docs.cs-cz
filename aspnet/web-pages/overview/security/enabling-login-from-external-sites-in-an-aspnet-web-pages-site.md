---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: "Přihlášení pomocí externí servery v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tento článek vysvětluje, jak se přihlásit na váš web ASP.NET Web Pages (Razor) pomocí služby Facebook, Google, Twitter, Yahoo a jiných lokalitách – to znamená, jak podporovat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Přihlášení pomocí externí servery v Web Pages (Razor) technologie ASP.NET
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak se přihlásit na váš web ASP.NET Web Pages (Razor) pomocí služby Facebook, Google, Twitter, Yahoo a jiných lokalitách – to znamená, jak ve vaší lokalitě podporovat OAuth a OpenID.
> 
> Získáte informace:
> 
> - Postup povolení přihlášení z jiných webů, když použijete šablonu Starter web služby WebMatrix.
> 
> Toto je funkce technologie ASP.NET byla zavedená v článku:
> 
> - `OAuthWebSecurity` Pomocné rutiny.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - Služba WebMatrix 3

Zahrnuje podporu pro ASP.NET Web Pages [OAuth](http://oauth.net/) a [OpenID](http://openid.net/) zprostředkovatele. Pomocí těchto poskytovatelů, můžete je nechat protokolu uživatele na váš web pomocí svých existujících přihlašovacích údajů ze sítě Facebook, Twitter, Microsoft a Google. K přihlášení pomocí účtu sítě Facebook, například uživatelé mohou zvolit právě Facebook ikonu, která je přesměruje na přihlašovací stránku služby Facebook, kde zadat informace o uživateli. Přihlášení k síti Facebook se pak můžete přidružit ke svému účtu na váš web. Související vylepšení funkcí členství webové stránky je, že uživatelé mohou přidružit více přihlášení (včetně přihlášení ze sociálních sítí) s jeden účet na vašem webu.

Tento obrázek zobrazuje stránku pro přihlášení z **Starter Site** šablony, které může uživatel Facebook, Twitter, Google nebo Microsoft ikonu povolíte protokolování s externím účtu:

![externí poskytovatelé](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Můžete povolit OAuth a OpenID členství uncommenting po zadání několika řádků kódu **Starter Site** šablony. Metody a vlastnosti, použijete pro práci s OAuth a OpenID poskytovatelé jsou v `WebMatrix.Security.OAuthWebSecurity` třídy. **Starter Site** Šablona zahrnuje úplné členství infrastruktury, s přihlašovací stránky, databáze členství a všechny kód, je třeba dát protokolu uživatele na váš web pomocí přihlašovacích údajů místního nebo ty z jiné lokality .

Tato část poskytuje příklad, jak umožnit uživatelům přihlášení z externí servery k lokalitě, která je založena na **Starter Site** šablony. Po vytvoření úvodní lokality, proveďte tento (podrobnosti použijte):

- Weby, které používají poskytovatele OAuth (Facebook, Twitter a Microsoft) vytvoříte aplikaci na webu externího. To vám dává klíče aplikace, které budete potřebovat k vyvolání funkce přihlášení pro tyto lokality.
- Weby, které používají poskytovatele OpenID (Google) není nutné k vytvoření aplikace. Pro všechny tyto servery musí mít účet k přihlášení a vytvořit vývojář aplikace.

    > [!NOTE]
    > Aplikace Microsoft přijímat pouze za provozu adresu URL pro funkčního webu, tak pro testování přihlášení nemůžete použít adresu URL místního webu.
- Chcete-li zadat poskytovatele příslušný ověřovací upravit několik souborů ve vašem webu a odeslat přihlášení k webu, kterou chcete použít.

Tento článek obsahuje samostatné pokyny pro následující úkoly:

- [Povolení přihlášení Google](#To_enable_Google_logins)
- [Povolení přihlášení Facebook](#To_enable_Facebook_logins)
- [Povolení přihlášení služby Twitter.](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Povolení přihlášení Google

1. Vytvořit nebo otevřít webové stránky ASP.NET Web založený na šabloně Starter web služby WebMatrix.
2. Otevřete  *\_AppStart.cshtml* stránky a zrušte komentář u následujícího řádku kódu. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testování Google přihlášení

1. Spustit *default.cshtml* stránku vašeho webu a zvolte **přihlásit** tlačítko.
2. Na *přihlášení* stránky v **přihlásit pomocí jiné služby** vyberte buď **Google** nebo **Yahoo** tlačítko Odeslat. Tento příklad používá přihlášení Google. 

    Webová stránka přesměruje požadavek na přihlašovací stránku Google.

    ![Google přihlášení](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Zadejte přihlašovací údaje pro existující účet Google.
4. Pokud Google dotazem, zda chcete povolit *Localhost* používat informace z účtu, klikněte na tlačítko **povolit**.

    Kód používá Google token k ověření uživatele a vrátí na tuto stránku na vašem webu. Tato stránka umožňuje uživatelům přidružení jejich Google přihlášení s existujícím účtem na vašem webu, nebo se můžete zaregistrovat nový účet na svém webu přidružit externí přihlášení s.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Vyberte **přidružit** tlačítko. V prohlížeči vrátí na domovskou stránku vaší aplikace.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Povolení přihlášení Facebook

1. Přejděte na [Facebook vývojáři lokality](https://developers.facebook.com/apps) (protokolu případě, že jste již přihlášeni).
2. Vyberte **vytvořit novou aplikaci** tlačítko a pak postupujte podle pokynů k pojmenování a vytvořit novou aplikaci.
3. V části **vyberte, jak bude vaše aplikace integrovat Facebook**, vyberte **webu** části.
4. Vyplňte **adresa URL webu** pole s adresou URL vašeho webu (například `http://www.example.com`). **Domény** pole je volitelné; můžete použít k ověření pro celou doménu (například *example.com*). 

    > [!NOTE]
    > Pokud používáte lokalitu v místním počítači s adresou URL jako `http://localhost:12345` (kde číslo je číslem místního portu), přidáním této hodnoty **adresa URL webu** pole pro testování vaší lokality. Ale kdykoli se číslo portu změny místní lokality, budete muset aktualizovat **adresa URL webu** pole vaší aplikace.
5. Vyberte **uložit změny** tlačítko.
6. Vyberte **aplikace** znovu a poté zobrazte úvodní stránky pro vaši aplikaci.
7. Kopírování **ID aplikace** a **tajný klíč aplikace** hodnoty pro vaši aplikaci a vložte je do dočasného textového souboru. Tyto hodnoty budou předat poskytovatele sítě Facebook v kódu webové stránky.
8. Ukončení webu pro vývojáře Facebook.

Teď provedete změny dvě stránky ve vašem webu tak, aby uživatelé budou moci přihlásit do lokality pomocí účtů služby Facebook.

1. Vytvořit nebo otevřít webové stránky ASP.NET Web založený na šabloně Starter web služby WebMatrix.
2. Otevřete  *\_AppStart.cshtml* stránky a zrušte komentář kódu pro zprostředkovatele OAuth pro Facebook. Blok uncommented kódu vypadá takto: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Kopírování **ID aplikace** hodnotu z aplikace Facebook jako hodnotu `appId` parametr (v uvozovkách).
4. Kopírování **tajný klíč aplikace** hodnotu z aplikace Facebook, jako `appSecret` hodnota parametru.
5. Soubor uložte a zavřete.

### <a name="testing-facebook-login"></a>Testování Facebook přihlášení

1. Spuštění tohoto webu *default.cshtml* stránce a vyberte položku **přihlášení** tlačítko.
2. Na *přihlášení* stránky v **přihlásit pomocí jiné služby** zvolte **Facebook** ikonu. 

    Webová stránka přesměruje požadavek na přihlašovací stránku služby Facebook.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Přihlaste se k účtu sítě Facebook. 

    Kód k vašemu ověření používá token služby Facebook a vrátí na stránky, kde je možné přidružit Facebook přihlašovací jméno vašeho webu přihlášení. Uživatelského jména nebo e-mailové adresy se naplní do **e-mailu** pole ve formuláři.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Vyberte **přidružit** tlačítko. 

    Vrátí prohlížeči na domovskou stránku a jste přihlášeni.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Povolení přihlášení služby Twitter.

1. Vyhledejte [Twitter vývojáři lokality](https://dev.twitter.com/).
2. Vyberte **vytvořit aplikaci** propojit a přihlaste se k webu.
3. Na **vytvořit aplikaci** formuláři, zadejte **název** a **popis** pole.
4. V **webu** pole, zadejte adresu URL vašeho webu (například `http://www.example.com`). 

    > [!NOTE]
    > Pokud testujete váš web místně (pomocí adresy URL jako `http://localhost:12345`), služby Twitter nemusí přijmout adresu URL. Nicméně je možné použít místní smyčky IP adresu (například `http://127.0.0.1:12345`). Tato funkce zjednodušuje proces testování aplikace s místně. Však pokaždé, když číslo portu místní lokality změní, budete muset aktualizovat **webu** pole vaší aplikace.
5. V **adresu URL zpětné volání** pole, zadejte adresu URL stránky ve vašem webu, kterou mají uživatelé se vraťte do po přihlášení do služby Twitter. Například uživatelé odeslat na domovskou stránku Starter Site (který rozpozná stav jejich přihlášení), zadejte stejnou adresu URL, kterou jste zadali v **webu** pole.
6. Přijměte podmínky a vyberte **vytvořit aplikaci služby Twitter** tlačítko.
7. Na **Moje aplikace** úvodní stránka, vyberte aplikaci, kterou jste vytvořili.
8. Na **podrobnosti** , posuňte se dolů a klikněte na příkaz **vytvořit Moje přístup Token** tlačítko.
9. Na **podrobnosti** kartě, zkopírujte **uživatelský klíč** a **uživatelský tajný klíč** hodnoty pro vaši aplikaci a vložte je do dočasného textového souboru. Tyto hodnoty budete předáte v kódu webové stránky k poskytovateli služby Twitter.
10. Ukončení webu služby Twitter.

Nyní můžete měnit dvě stránky ve vašem webu tak, aby uživatelé budou moci přihlásit do lokality pomocí účtů služby Twitter.

1. Vytvořit nebo otevřít webové stránky ASP.NET Web založený na šabloně Starter web služby WebMatrix.
2. Otevřete  *\_AppStart.cshtml* stránky a zrušte komentář kódu pro zprostředkovatele služby Twitter OAuth. Blok uncommented kódu vypadá takto: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Kopírování **uživatelský klíč** hodnotu z aplikace služby Twitter jako hodnotu `consumerKey` parametr (v uvozovkách).
4. Kopírování **uživatelský tajný klíč** hodnotu z aplikace služby Twitter jako hodnotu `consumerSecret` parametr.
5. Soubor uložte a zavřete.

### <a name="testing-twitter-login"></a>Testování přihlášení služby Twitter.

1. Spustit *default.cshtml* stránku vašeho webu a zvolte **přihlášení** tlačítko.
2. Na *přihlášení* stránky v **přihlásit pomocí jiné služby** zvolte **Twitter** ikonu. 

    Webová stránka přesměruje požadavek na přihlašovací stránku služby Twitter pro aplikaci, kterou jste vytvořili.

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Přihlaste se k účtu sítě Twitter.
4. Kód používá token služby Twitter. k ověření uživatele a vrátí můžete na stránky, kde můžete přidružit vaše přihlášení pomocí účtu webu. Jméno nebo e-mailovou adresu se naplní do **e-mailu** pole ve formuláři.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Vyberte **přidružit** tlačítko. 

    Vrátí prohlížeči na domovskou stránku a jste přihlášeni.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


- [Přizpůsobení chování na webu](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Přidání členství a zabezpečení do stránky webu technologie ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
