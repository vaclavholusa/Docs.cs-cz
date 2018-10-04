---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure ověřování | Dokumentace Microsoftu
author: Rick-Anderson
description: Microsoft ASP.NET tools pro Windows Azure Active Directory umožňuje snadno zajistit ověřování pro webové aplikace hostované na Windows Azure Web Sites...
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: a45b0ad2b61c2b78f7f06e85fe5e92193d73041d
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577506"
---
<a name="windows-azure-authentication"></a>Windows Azure ověřování
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Microsoft ASP.NET tools pro Windows Azure Active Directory usnadňuje povolení ověřování pro webové aplikace hostované na [weby Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Ověřování Windows Azure můžete použít k ověření uživatelů Office 365 z vaší organizace, podnikové účty synchronizované z vaší místní Active Directory nebo uživatelé vytvoření ve vlastní doméně Windows Azure Active Directory. Když se povolí ověřování Windows Azure nakonfiguruje vaše aplikace k ověřování uživatelů pomocí jediného [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenanta.
> 
> Ověřování ASP.NET pomocí Windows Azure nástroj není podporován pro webové role v cloudové službě, ale plánujeme v některé budoucí verzi. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) se podporuje ve webových rolí Windows Azure.
> 
> Podrobnosti o tom, jak nastavit synchronizaci mezi vaší místní Active Directory a klienta služby Windows Azure Active Directory najdete v tématu [pomocí služby AD FS 2.0 k implementaci a správě jednotného přihlašování](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory je aktuálně k dispozici jako [bezplatná služba ve verzi preview](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Požadavky:

- Visual Studio 2012 nebo [sady Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Webové rozšíření pro sadu Visual Studio 2012 nástrojů](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) nebo [Web Tools Extensions pro sadu Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) nebo [technologii Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 pro Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Vytvoření webové aplikace ASP.NET pomocí sady Visual Studio 2012

Můžete vytvořit libovolné webové aplikace pomocí sady Visual Studio 2012, tento kurz používá intranetovou šablonu ASP.NET MVC.

1. Vytvoření nové aplikace ASP.NET MVC 4 intranetu a přijměte všechny výchozí hodnoty. (Musí to být In **tra** net a nikoli v **Uko** net projektu).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Povolit ověřování Azure (Pokud jste globálním správcem principem)

Pokud nemáte existujícího tenanta služby Windows Azure Active Directory (například přes stávající účet služeb Office 365) můžete vytvořit nového tenanta když si zaregistrujete [nový účet služby Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. V nabídce projektu zvolte **povolit ověřování Windows Azure**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Zadejte doménu pro tenanta služby Windows Azure Active Directory (například contoso.onmicrosoft.com) a klikněte na tlačítko **povolit**:

![](windows-azure-authentication/_static/image3.png)

3. Ve webové ověřování dialogové okno přihlášení jako správce pro vašeho tenanta služby Windows Azure Active Directory:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Povolit okno Azure bez oprávnění správce zásada

Pokud nemáte oprávnění globálního správce pro vašeho tenanta služby Windows Azure Active Directory, můžete zrušit zaškrtnutí políčka pro zřizování aplikace.

![](windows-azure-authentication/_static/image6.png)

Dialogové okno se zobrazí **domény**, **Id objektu zabezpečení aplikace** a **adresy URL odpovědi** vyžadované pro zřizování aplikace s Azure Active Directory zásada. Budete muset poskytnout tyto informace k někdo, kdo má dostatečná oprávnění ke zřízení aplikace. Zobrazit[implementace jednotné přihlašování s Windows Azure Active Directory – aplikace ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) podrobnosti o tom, jak můžete vytvořit instanční objekt ručně pomocí rutiny.  
Jakmile se aplikace po úspěšném zřízení, můžete kliknout na **i nadále aktualizovat soubor web.config s vybraným nastavením**. Pokud chcete pokračovat ve vývoji aplikací při čekání na zřízení nestane, můžete kliknout na **zavřít pamatovat nastavení v souboru projektu**. Při příštím volání povolit ověřování Windows Azure a zrušte zaškrtnutí políčka zřizování, zobrazí se stejná nastavení a můžete kliknout na **pokračovat**, klepněte na tlačítko **použít tato nastavení v souboru web.config**.

1. Počkejte, dokud vaše aplikace je nakonfigurovaná pro ověřování Windows Azure a zřízení pomocí služby Windows Azure Active Directory.
2. Po povolení ověřování Windows Azure pro vaši aplikaci klikněte na tlačítko **Zavřít:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Stisknutím klávesy F5 spusťte aplikaci. Byste měli získat automaticky přesměrováni na přihlašovací stránku. Použijte přihlašovací údaje uživatele adresáře principem se přihlásit k aplikaci...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Protože aplikace aktuálně používá certifikát podepsaný svým držitelem testů obdrží upozornění z prohlížeče, který certifikát nebyl vydán důvěryhodnou certifikační autoritou.

    Toto upozornění můžete ignorovat během místního vývojového kliknutím **pokračovat na tento web:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Jste teď úspěšně přihlásí k aplikaci pomocí ověřování Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Povolení Windows Azure umožňuje ověřování následující změny do vaší aplikace:

- Padělání žádosti mezi proti-Site ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) třídy ( *aplikace\_Start\AntiXsrfConfig.cs* ) se přidá do vašeho projektu.
- Balíčky NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` se přidá do vašeho projektu.
- Nastavení Windows Identity Foundation ve vaší aplikaci se nakonfigurují tak, aby přijímal tokeny zabezpečení z vašeho tenanta služby Windows Azure Active Directory. Klikněte na obrázku níže můžete vidět rozbalené zobrazení změn provedených *Web.config* souboru.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Objekt služby pro vaši aplikaci ve vašem tenantovi služby Windows Azure Active Directory se zřídí.
- Je povolenou komunikací HTTPS.

## <a name="deploy-the-application-to-windows-azure"></a>Nasazení aplikace do Windows Azure

Úplné pokyny najdete v tématu [nasazení webové aplikace ASP.NET na web Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Publikování aplikace prostřednictvím ověřování Windows Azure na web Azure:

1. Klikněte pravým tlačítkem na svou aplikaci a vyberte **publikovat:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. V dialogovém okně Publikovat Web stáhnout a importovat profil publikování pro svůj web Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. **Připojení** zobrazuje kartu **cílovou adresu URL** (veřejný internetový URL pro vaši aplikaci). Klikněte na tlačítko **ověřit připojení** abyste připojení dál otestovali:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Pokud na tento web Azure, které jste publikovali dříve zvažte kontrolu **odebrat další soubory v cílovém umístění** nastavení, aby vaše aplikace publikuje čistě. Všimněte si, že **povolit ověřování Windows Azure** zaškrtávací políčko je padesáti.  

    ![](windows-azure-authentication/_static/image10.png)
5. Volitelné: Na **ve verzi Preview** klikněte na kartu **spustit Náhled** zobrazíte soubory, které jsou nasazené.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Klikněte na tlačítko **publikovat.**

    Zobrazí výzva k povolení ověřování Windows Azure pro cílového hostitele. Klikněte na tlačítko **povolit** pokračovat:

    ![](windows-azure-authentication/_static/image11.png)
7. Zadejte svoje přihlašovací údaje správce pro vašeho tenanta služby Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Jakmile vaše aplikace se úspěšně publikovala, otevře se prohlížeč k publikované webové stránky.

    > [!NOTE]
    > Může trvat až pět minut (obvykle musí méně) pro vaši aplikaci do jejich kompletní zřízení včetně Windows Azure Active Directory po povolení ověřování Windows Azure pro cílového hostitele. Při prvním spuštění vaší aplikace. Pokud se zobrazí chyba ACS50001: předávající strana s názvem [sféry] nebyl nalezen, počkejte několik minut a zkuste aplikaci znovu spustit.
9. Po zobrazení výzvy, přihlaste se jako uživatel ve vašem adresáři:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Jste teď úspěšně přihlášení Azure hostované aplikace s použitím ověřování Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Známé problémy

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Autorizace na základě rolí selže, pokud používáte ověřování Windows Azure < o:p >< / o:p >

Ověřování Windows Azure aktuálně neposkytuje deklarace potřebné role tak, aby ověřování na základě rolí lze provést. Role ověřeného uživatele, musíte ručně načíst z Windows Azure Active Directory. < o:p >< / o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Procházení k aplikaci s výsledky ověřování Windows Azure v chybě "ACS20016 domény přihlášeného uživatele (live.com) se neshoduje s všechny povolené domény této služby tokenů zabezpečení" < o:p >< / o:p >

Pokud jste již přihlášeni k Account Microsoft (například hotmail.com, live.com, outlook.com) a při pokusu o přístup k aplikaci, která má povolené ověřování Windows Azure může získat odpověď 400 chyb, protože domény Account Microsoft Windows Azure Active Directory není rozpoznána. K přihlášení do aplikace, odhlaste se z Account Microsoft nejprve. < o:p >< / o:p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Protokolování do aplikace s povoleným ověřováním Windows Azure a X509CertificateValidationMode jiné než žádný má za následek chyby ověření certifikátu pro certifikát accounts.accesscontrol.windows.net < o:p >< / o:p >

Ověření certifikátu se nevyžaduje a by měla zůstat zakázáno. Kryptografický otisk vystavitele certifikátu je ověřené WSFederationAuthenticationModule. < o:p >< / o:p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Při pokusu o povolení ověřování Windows Azure Web ověřovacím dialogovém okně zobrazí chybu "ACS20016: domény přihlášeného uživatele (contoso.onmicrosoft.com) se neshoduje s všechny povolené domény této služby tokenů zabezpečení." < o:p >< / o:p >

Tato chyba může zobrazit, když jste už dřív úspěšně přihlášení pomocí jiného účtu služby Windows Azure Active Directory z v rámci stejného procesu Visual Studio. Odhlásit ze zadaného účtu nebo restartujte sadu Visual Studio. Pokud jste dříve přihlášení a vybrali možnost "Neodhlašovat", pak budete muset vymazat soubory cookie prohlížeče. < o:p >< / o:p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Požadavek není platný zprávy protokolu WS-Federation < o:p >< / o:p >

To může nastat, pokud jste už přihlášení pomocí některé jiné ID Microsoft na jednu ze služeb Azure. Použití privátní okno prohlížeče v Internet Exploreru InPrivate nebo Incognito v prohlížeči Chrome, jako je nebo zrušte všechny soubory cookie. <o:p></o:p>

## <a name="additional-resources"></a>Další prostředky

- [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows funkce Azure: Identity](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: vývoj aplikací pro vaši organizaci](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: vývoj aplikací pro víc organizací](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Implementace jednotné přihlašování s Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Jednotné přihlašování s Windows Azure Active Directory: podrobné informace o](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Pomocí služby AD FS 2.0 k implementaci a správě jednotného přihlašování](https://technet.microsoft.com/library/jj205462.aspx)
