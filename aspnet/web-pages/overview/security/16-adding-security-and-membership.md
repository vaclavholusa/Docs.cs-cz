---
uid: web-pages/overview/security/16-adding-security-and-membership
title: "Přidání členství a zabezpečení do rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tato kapitola se dozvíte, jak zabezpečit svůj web tak, aby některé stránky jsou k dispozici jenom uživatelům přihlásit. (Dozvíte se taky, jak vytvořit stránky zadaná..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: af2eeb128cff554e7ae3d903e2117861087344e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Přidání členství a zabezpečení na web rozhraní ASP.NET Web Pages (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak k zabezpečení webu k rozhraní ASP.NET Web Pages (Razor), aby některé stránky k dispozici pouze na uživatele, kteří přihlásit. (Také uvidíte postup vytvoření stránky, které můžete přístup kýmkoli.)
> 
> **Získáte informace:** 
> 
> - Jak vytvořit web, který má na stránce registrace a přihlašovací stránku tak, aby pro některé stránky můžete omezit přístup k pouze členové.
> - Jak vytvořit veřejný a pouze stránky.
> - Postup definování rolí, které jsou skupiny, které mají oprávnění různých zabezpečení na svém webu, a jak přiřadit uživatele k roli.
> - Jak používat CAPTCHA zabránit ve vytváření účtů člen automatizovaným programům (robotů).
>   
> 
> Toto jsou nové v článku funkce ASP.NET:
> 
> - WebMatrix **Starter Site** šablony.
> - `WebSecurity` Pomocné rutiny a `Roles` třídy.
> - `ReCaptcha` Pomocné rutiny.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - Služba WebMatrix 3
> - ASP.NET – webové pomocné knihovny


Váš web můžete nastavit tak, aby uživatelé mohli přihlásit do Vyb &#8212; To znamená, že lokalita podporuje *členství*. To může být užitečné z mnoha důvodů. Například možná váš web stránek, které by měly být dostupné pouze pro členy. V některých případech může vyžadovat, aby bylo možné je odeslat zpětnou vazbu nebo komentář přihlášení uživatelů.

I v případě, že váš web podporuje členství, uživatelé nejsou nezbytně potřeba přihlášení, než používají některé stránky na webu. Uživatelé, kteří nejsou přihlášeni se označují jako *anonymní uživatelé*.

Uživatele můžete zaregistrovat na vašem webu a můžete pak přihlásit k webu. Tento web vyžaduje uživatelské jméno (e-mailovou adresu) a heslo k potvrzení, že uživatelé jsou koho se být. Tento proces přihlášení a potvrzení identity uživatele se označuje jako *ověřování*.

Nastavením členství a zabezpečení různými způsoby:

- Pokud používáte službu WebMatrix, snadný způsob, je vytvořit jako nový web na základě **Starter Site** šablony. Tato šablona už je nakonfigurovaný pro členství a zabezpečení a již má registrační stránku, přihlašovací stránku a tak dále.

    Lokality vytvořených šablonou má také možnost nechat uživatele přihlásit pomocí externího webu jako je Facebook, Google, nebo služby Twitter.
- Pokud chcete zvýšit zabezpečení na existující web, nebo pokud nechcete použít **Starter Site** šablony, můžete vytvořit vlastní registrační stránce, přihlašovací stránku a tak dále.

Tento článek zaměřuje na první možnost &mdash; postup přidání zabezpečení pomocí **Starter Site** šablony. Také poskytuje některých základních informací o tom, jak implementovat vlastní zabezpečení a odkazy na další informace o tom, jak to udělat. Je zde také informace o tom, jak povolit externí přihlášení, který je popsán v podrobněji věnovaný samostatný článek.

## <a name="creating-website-security-using-the-starter-site-template"></a>Vytváření pomocí šabloně Starter Site zabezpečení webu

Ve službě WebMatrix, můžete použít **Starter Site** šablony k vytvoření webu, který obsahuje následující:

- Databáze, která se používá k ukládání uživatelských jmen a hesel pro členy.
- Na stránce registrace, kde můžete zaregistrovat anonymní uživatelé (Nový).
- Na stránce přihlášení a odhlášení.
- Heslo pro obnovení a resetování stránka.

Následující postup popisuje, jak vytvořit webu a nakonfigurovat ho.

1. Spusťte službu WebMatrix a v **rychlý Start** vyberte **šablonu z webu**.
2. Vyberte **Starter Site** šablony a pak klikněte na tlačítko **OK**. Služba WebMatrix vytvoří nový web.
3. V levém podokně klikněte **soubory** selektor pracovního prostoru.
4. V kořenové složce vašeho webu, otevřete  *\_AppStart.cshtml* souboru, který představuje speciální soubor, který slouží jako globální nastavení. Obsahuje některé příkazy, které jsou vloženy do komentáře pomocí `//` znaků:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Konfigurace těchto příkazů `WebMail` pomocné rutiny, které slouží k odesílání e-mailů. Systém členství slouží e-mailu k odesílání zpráv potvrzení, když uživatelé zaregistrovat nebo když chtějí ke změně hesla. (Například po registraci uživatelů, dostanou e-mailu, který zahrnuje odkaz, který můžete kliknout na aby bylo možné dokončit proces registrace.)

    Odesílání e-mailu vyžaduje přístup k serveru SMTP, jak je popsáno v [přidání e-mailu na web ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202899). Nastavení e-mailu budete ukládat v této střed  *\_AppStart.cshtml* tak, aby nemáte kódu opakovaného na každé stránce, který může odesílat e-mailu. (Nemusíte konfigurace nastavení protokolu SMTP k nastavení databáze registrace, potřebujete jenom nastavení SMTP, pokud chcete ověřit uživatele z jejich e-mailový alias a umožňují uživatelům obnovit zapomenuté heslo.)
5. Zrušením komentáře u příkazy odebráním `//` from in front of každé z nich.

    Pokud nechcete nastavit potvrzení e-mailu, můžete přeskočit tento krok a dalším krokem. Pokud nejsou nastavené hodnoty SMTP, nový účet je ihned k dispozici bez e-mailu s potvrzením.
6. Upravte následující nastavení související s e-mailu v kódu:

    - Nastavit `WebMail.SmtpServer` na název serveru SMTP, který máte přístup.
    - Nechte `WebMail.EnableSsl` nastavena na `true`. Toto nastavení slouží k zabezpečení přihlašovacích údajů, které se odesílají na SMTP server šifrováním.
    - Nastavit `WebMail.UserName` na uživatelské jméno pro svůj účet serveru SMTP.
    - Nastavit `WebMail.Password` na heslo pro svůj účet serveru SMTP.
    - Nastavit `WebMail.From` vlastní e-mailovou adresu. Toto je e-mailovou adresu, které je zpráva odeslána z.

    > [!NOTE] 
    > 
    > **Tip** Další informace o hodnotách těchto vlastností najdete v tématu [konfigurace nastavení e-mailu](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) v [přizpůsobení chování na webu pro webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Uložte a zavřete  *\_AppStart.cshtml*.
8. Spustit *Default.cshtml* stránku v prohlížeči.

    ![security-membership-2](16-adding-security-and-membership/_static/image1.png)

    > [!NOTE]
    > Pokud se zobrazí chyba oznamující, že vlastnost musí být instance `ExtendedMembershipProvider`, nemusí být konfigurována webu použít systém členství technologie ASP.NET Web Pages (SimpleMembership). Někdy může dojít v případě poskytovatele hostingu server je nakonfigurován jinak než místní server. Pokud chcete odstranit tento problém, přidejte následující element do lokality *Web.config* souboru:
    > 
    > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
    > 
    > Přidejte tento prvek jako podřízenou `<configuration>` elementu a jako partnerské zařízení `<system.web>` elementu.
9. V pravém horním rohu stránky, klikněte na **zaregistrovat** odkaz. *Register.cshtml* zobrazí se stránka.
10. Zadejte uživatelské jméno a heslo a potom klikněte na **zaregistrovat**.

    ![security-membership-3](16-adding-security-and-membership/_static/image2.png)

    Při vytváření webu z **Starter Site** šablony, databáze s názvem *StarterSite.sdf* byl vytvořen na webu *aplikace\_Data* složky. Informace o uživateli se při registraci, přidá do databáze. Pokud nastavíte hodnoty SMTP, je odeslána zpráva e-mailovou adresu, které že jste použili, můžete dokončit registraci.

    ![security-membership-4](16-adding-security-and-membership/_static/image3.png)
11. Přejděte do programu vaše e-mailu a vyhledejte zpráva, což bude mít potvrzovací kód a hypertextový odkaz na web.
12. Klikněte na odkaz aktivovat svůj účet. Potvrzení hypertextový odkaz otevře potvrzovací stránku registrace.

    ![security-membership-5](16-adding-security-and-membership/_static/image4.png)
- Klikněte **přihlášení** propojit a pak se přihlaste pomocí účtu, který je zaregistrovaný.

    Po přihlášení se **přihlášení** a **zaregistrovat** se nahrazují odkazy **odhlášení** odkaz. Vaše přihlašovací jméno se zobrazí jako odkaz. (Tento odkaz umožňuje přejděte na stránku, kde můžete změnit heslo.)

    ![security-membership-6](16-adding-security-and-membership/_static/image5.png)

    > [!NOTE]
    > Ve výchozím nastavení rozhraní ASP.NET web pages poslat přihlašovací údaje serveru ve formátu prostého textu (jako čitelný text). Produkční lokality by měl používat zabezpečený protokol HTTP (https://, také známé jako *zabezpečení SSL* nebo SSL) k šifrování citlivých informací, které se vyměňují se serverem. Můžete požadované e-mailu odesílání zpráv pomocí protokolu SSL nastavením `WebMail.EnableSsl=true` jako v předchozím příkladu. Další informace o SSL najdete v tématu [zabezpečení komunikace webových: certifikáty SSL a https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Funkce další členství v této lokalitě

Váš web obsahuje další funkce, která umožňuje uživatelům spravovat své účty. Uživatele můžete provést následující:

- Změňte své heslo. Po přihlášení, že klikněte na uživatelské jméno (což je odkaz). Tato akce trvá je na stránku, kde můžete vytvořit nové heslo (*Account/ChangePassword.cshtml*).
- Obnovte zapomenuté heslo. Na stránce přihlášení je odkaz (**zapomněli jste heslo?**), která má uživatele na stránku (*Account/ForgotPassword.cshtml*) kde můžou zadat e-mailovou adresu. Lokality je odešle e-mailovou zprávu, která má odkaz, který můžete kliknout na Chcete-li nastavit nové heslo (*Account/PasswordReset.cshtml*).

Můžete taky umožnit uživatelům přihlásit se pomocí externího webu, může taky, jak je popsáno později.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Vytvoření stránky pouze pro členy

Prozatím, každý, kdo můžete vyhledat jakoukoli stránku ve vašem webu. Ale můžete chtít mít stránek, které jsou k dispozici pouze pro uživatele, kteří se přihlásí (to znamená, členům). Technologie ASP.NET umožňuje vytvářet stránky, které jsou přístupné pouze pomocí přihlášeného členy. Obvykle Pokud anonymní uživatelé se pokusí přistoupit ke stránce pouze pro členy, je přesměruje je na přihlašovací stránku.

V tomto postupu vytvoříte složku, která bude obsahovat stránky, které jsou k dispozici pouze pro přihlášeného uživatele.

1. V kořenovém adresáři serveru vytvořte novou složku. (Na pásu karet klikněte na šipku pod **nový** a potom zvolte **novou složku**.)
2. Název nové složky *členy*.
3. Uvnitř *členy* složku, vytvořte novou stránku a s názvem ho *MembersInformation.cshtml*.
4. Nahraďte existující obsah následující kód a značky:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Tento kód testy `IsAuthenticated` vlastnost `WebSecurity` objekt, který vrátí `true` Pokud má uživatel přihlášen. Pokud uživatel není přihlášen, kód zavolá metodu `Response.Redirect` odeslat uživatelům *Login.cshtml* stránku *účtu* složky.

    Obsahuje adresu URL přesměrování `returnUrl` dotaz řetězcovou hodnotu, která používá `Request.Url.LocalPath` nastavit cestu k aktuální stránce. Pokud nastavíte `returnUrl` hodnota v řetězci dotazu takto (a pokud návratová adresa URL je místní cesta), přihlašovací stránku vrátí uživatele na tuto stránku po přihlášení.

    Kód také nastaví  *\_SiteLayout.cshtml* stránce jako jeho rozložení stránky. (Další informace o rozložení stránky najdete v tématu [vytváření konzistentního rozložení v lokalitách webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Spuštění tohoto webu. Pokud jste stále přihlášeni, klikněte **odhlášení** tlačítka v horní části stránky.
6. V prohlížeči, požadavek stránku */členy/MembersInformation*. Adresa URL může například vypadat například takto:

    `http://localhost:38366/Members/MembersInformation`

    (Číslo portu (38366) bude pravděpodobně lišit v svoji adresu URL.)

    Budete přesměrováni na *Login.cshtml* stránky, protože nejste přihlášeni.
- Přihlaste se pomocí účtu, který jste vytvořili dříve. Budete přesměrováni zpět *MembersInformation* stránky. Vzhledem k tomu, že jste přihlášeni, tentokrát uvidíte obsah stránky.

Zabezpečený přístup k více stránek, můžete to udělat:

- Kontrola zabezpečení přidáte na každou stránku.
- Vytvoření  *\_PageStart.cshtml* stránku ve složce, kde můžete ponechat chráněné stránky a přidat kontrola zabezpečení existuje. *\_PageStart.cshtml* stránky funguje jako globální stránky pro všechny stránky ve složce. Tento postup je vysvětlené podrobněji v [přizpůsobení chování na webu pro webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Vytváření zabezpečení pro skupiny uživatelů (role)

Pokud vaše lokalita zahrnuje mnoho členů, není efektivní předtím, než je zobrazit stránka s necháte jednotlivě zkontrolujte oprávnění pro každého uživatele. Co můžete dělat místo toho je k vytváření skupin, nebo *role*, že jednotlivé členy patří do. Potom můžete zkontrolovat oprávnění na základě role. V této části vytvoříte &quot;správce&quot; role a poté vytvořit stránky, který je přístupný pro uživatele, kteří jsou v (patřící k) této role.

K podpoře rolí je nastavený systém členství technologie ASP.NET. Ale na rozdíl od členství registrace a přihlášení **Starter Site** šablona neobsahuje stránek, které vám pomohou spravovat role. (Správa rolí je Správce úloh, nikoli úlohu uživatele.) Můžete však přidat skupiny přímo v databázi členství ve službě WebMatrix.

1. Ve službě WebMatrix, klikněte **databáze** selektor pracovního prostoru.
2. V levém podokně, otevřete *StarterSite.sdf* uzlu, otevřete **tabulky** uzel a poté dvojitým kliknutím *webové stránky\_role* tabulky.

    ![security-membership-7](16-adding-security-and-membership/_static/image6.png)
3. Přidání role s názvem &quot;správce&quot;. *RoleId* pole se vyplní automaticky. (Je primární klíč a byla nastavena na být pole zjistěte, jak je popsáno v [Úvod k práci s databází v lokalitách webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Poznamenejte si hodnotu Novinky pro *RoleId* pole. (Pokud je to první role, které definujete, bude 1.)

    ![security-membership-8](16-adding-security-and-membership/_static/image7.png)
5. Zavřít *webové stránky\_role* tabulky.
6. Otevřete *UserProfile* tabulky.
7. Poznamenejte si *UserId* hodnota jednoho nebo více uživatelů v tabulce a pak zavřete v tabulce.
8. Otevřete *webové stránky\_UserInRoles* tabulku a zadejte *UserID* a *RoleID* hodnotu do tabulky. Například put uživatel 2 do &quot;správce&quot; role, zadejte tyto hodnoty:

    ![security-membership-9](16-adding-security-and-membership/_static/image8.png)
9. Zavřít *webové stránky\_UsersInRoles* tabulky.

    Teď, když máte role, které definují, můžete nakonfigurovat na stránce, který je přístupný pro uživatele, kteří jsou v dané roli.
10. V kořenové složce webu vytvořte novou stránku s názvem *AdminError.cshtml* a nahradí existující obsah následujícím kódem. Bude stránka, která uživatelé přesměrovaní na Pokud se nebudou mít přístup ke stránce.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. V kořenové složce webu vytvořte novou stránku s názvem *AdminOnly.cshtml* a existujícího kódu nahraďte následujícím kódem:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    `Roles.IsUserInRole` Metoda vrátí `true` Pokud má aktuální uživatel je členem zadané roli (v tomto případě roli "admin").
12. Spustit *Default.cshtml* v prohlížeči, ale nemáte přihlásit. (Pokud jste již přihlášeni, odhlaste.)
13. V panelu Adresa prohlížeče, přidejte *AdminOnly* v adrese URL. (Jinými slovy, požadavků *AdminOnly.cshtml* souboru.) Budete přesměrováni na *AdminError.cshtml* stránky, protože nejste přihlášeni aktuálně jako uživatel v &quot;správce&quot; role.
14. Vraťte se do *Default.cshtml* a přihlaste se jako uživatel, který jste přidali do &quot;správce&quot; role.
15. Přejděte do *AdminOnly.cshtml* stránky. Tentokrát naleznete na stránce.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Brání automatizovaným programům připojení webu

Na přihlašovací stránku neukončí automatizovaným programům (někdy označovány jako *webové roboti* nebo *robotů*) registraci vašeho webu. Tento postup popisuje, jak povolit nástroje ReCaptcha testu na stránce registrace.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Zaregistrovat váš web v ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po dokončení registrace budete získat veřejný klíč a soukromý klíč.
2. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.
3. V *účet* složku, otevřete soubor s názvem *Register.cshtml*.
4. V kódu v horní části stránky, vyhledejte následující řádky a Odkomentujte jejich odebráním `//` komentář znaků:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Nahraďte `PRIVATE_KEY` pomocí vlastního soukromého klíče nástroje ReCaptcha.
6. V kódu stránky, odebrat `@*` a `*@` komentovaný znaky z kolem následující řádky do kódu stránky:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Nahraďte `PUBLIC_KEY` váš klíč.
8. Pokud už je nebyly odebrat, odeberte `<div>` element, který obsahuje text, který začíná "Povolit ověřování CAPTCHA...". (Odeberte celý `<div>` elementu a jeho obsah.)

1. Spustit *Default.cshtml* v prohlížeči. Pokud jste se přihlásili do lokality, klikněte **odhlášení** odkaz.
2. Klikněte **zaregistrovat** propojení a testování registraci pomocí testu CAPTCHA.

    ![security-membership-10](16-adding-security-and-membership/_static/image9.png)

Další informace o `ReCaptcha` pomocné rutiny, najdete v části [pomocí CATPCHA zabránit automatizované programy (robotů) z pomocí vašeho webu ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Umožňuje uživatelům přihlásit pomocí externího webu

**Starter Site** šablona obsahuje kód a kód, který umožňuje uživatelům přihlášení pomocí Facebooku, Windows Live, službami Twitter, Google a Yahoo. Tato funkce není ve výchozím nastavení povolené. Obecný postup pro používání povolení uživatelům přihlásit se pomocí tyto externí zprostředkovatele je toto:

- Rozhodnete, které externí servery chcete podporovat.
- V případě potřeby přejít na server a nastavit aplikaci přihlášení. (Například máte k tomu, aby bylo možné povolit Facebook přihlášení.)
- Ve vaší lokalitě nakonfigurujte zprostředkovatele. Ve většině případů musíte se zrušte komentář u některých kód  *\_AppStart.cshtml* souboru.
- Přidání značek k registrační stránce, která umožní uživatelům odkaz na externí web pro přihlášení. Obvykle můžete zkopírovat kód potřebujete a změňte text mírně.

Podrobné pokyny najdete v tématu [povolení přihlášení z externí lokality v lokalitě služby ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969).

Jakmile se uživatel přihlásí z jiné lokality, vrátí uživatele na váš web a *přidruží* tohoto přihlášení s webem. To platí, vytvoří záznam členství ve vaší lokalitě pro externí přihlášení uživatele. To vám umožní používat normální zařízení členství (například role) s externí přihlášení.

## <a name="adding-security-to-an-existing-website"></a>Přidání zabezpečení na existující web

Využívá pomocí postupu v tomto článku výše **Starter Site** šablony jako základ pro zabezpečení webu. Pokud není praktické pro spuštění z **Starter Site** šablony nebo zkopírovat příslušné stránky z lokality na základě této šablony, můžete implementovat stejný typ zabezpečení ve vaší vlastní lokalitě pomocí kódování sami. Vytvořit stejné typy stránky – registrace, přihlášení a tak dále – a potom nastavit členství pomocí třídy a pomocné rutiny.

Základní proces je popsaný v příspěvku na blogu [nejzákladnější možnost pro implementaci zabezpečení ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). Většinu práce se provádí pomocí následujících metod a vlastností `WebSecurity` pomocné rutiny:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Tyto metody umožňují určit, zda uživatel je již zaregistrován a jejich registrace.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Tato vlastnost umožňuje určit, zda má aktuální uživatel je přihlášen. To je užitečné přesměrovat uživatele na přihlašovací stránku, pokud již uživatel není přihlášený.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Tyto metody se uživatel přihlásil příchozí nebo odchozí.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Tato vlastnost je užitečná pro zobrazení aktuálního uživatele přihlášeného název (Pokud je uživatel přihlášen).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Tato metoda je užitečná, pokud jste nastavili potvrzení e-mailu pro registraci. (Podrobnosti jsou popsány v příspěvku na blogu [používá funkci potvrzení pro zabezpečení rozhraní ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Ke správě rolí, můžete použít [role](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) a [členství](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) třídy, jak je popsáno v položkách blogu.

## <a name="additional-resources"></a>Další prostředky

- [Přizpůsobení chování v celém webu](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Zabezpečení komunikace webových: Certifikáty, SSL a https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [Nejzákladnější možnost pro implementaci zabezpečení ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) a [používá funkci potvrzení pro zabezpečení rozhraní ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Toto jsou příspěvky blogu, které popisují, jak implementovat funkce členství technologie ASP.NET bez použití **Starter Site** šablony.
- [Povolení přihlášení z externích webů na webu s webovými stránkami ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Referenční dokumentace rozhraní API třídy WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Referenční dokumentace rozhraní API třídy SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Referenční dokumentace rozhraní API třídy SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
