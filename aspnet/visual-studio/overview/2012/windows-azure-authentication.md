---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure ověřování | Microsoft Docs
author: Rick-Anderson
description: Nástroje Microsoft ASP.NET tools pro systém Windows Azure Active Directory usnadňuje povolení ověřování pro webové aplikace hostované na weby systému Windows Azure...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 09cb37ceb0132958a48f5f3a5d52dc46c6f0a78d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874821"
---
<a name="windows-azure-authentication"></a>Windows Azure ověřování
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> Nástroje Microsoft ASP.NET pro Windows Azure Active Directory usnadňuje povolení ověřování pro webové aplikace hostované na [weby systému Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Ověřování systému Windows Azure můžete použít k ověření uživatele služeb Office 365 z vaší organizace, podnikové účty synchronizované z vaší místní službou Active Directory nebo uživatelé vytvoření ve vlastní domény systému Windows Azure Active Directory. Povolení ověřování systému Windows Azure nakonfiguruje aplikace k ověření uživatelů pomocí jedné [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) klienta.
> 
> Nástroj ASP.NET ověřování systému Windows Azure se nepodporuje pro webové role v cloudové službě ale plánujeme Uděláte to tak v budoucí verzi. [Technologie Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) je podporována v systému Windows Azure webové role.
> 
> Další informace o tom, jak nastavit synchronizaci mezi vaší místní službou Active Directory a klienta služby Windows Azure Active Directory naleznete [pomocí služby AD FS 2.0 k implementaci a správě jednotného přihlašování](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory je momentálně k dispozici jako [preview služba úrovně free](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Požadavky:

- Visual Studio 2012 nebo [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Rozšíření pro sadu Visual Studio 2012 nástrojů Web](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) nebo [Web Tools Extensions pro sadu Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Nástroje Microsoft ASP.NET pro systém Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) nebo [nástroje Microsoft ASP.NET pro systém Windows Azure Active Directory – Visual Studio Express 2012 pro Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Vytvoření webové aplikace ASP.NET pomocí sady Visual Studio 2012

Všechny webové aplikace můžete vytvořit pomocí sady Visual Studio 2012, tento kurz používá intranetovou šablonu ASP.NET MVC.

1. Vytvoření nové aplikace ASP.NET MVC 4 intranetu a přijměte všechny výchozí hodnoty. (Musí být In **funkce tra** sítě a nikoli v **dejte** net projektu).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Povolit ověřování Azure okno (Pokud jste globálním správcem principem)

Pokud nemáte existujícího klienta služby Windows Azure Active Directory (například prostřednictvím existující účet Office 365) můžete vytvořit nového klienta registrujete [nový účet služby Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. Vyberte z nabídky projektu **povolit ověřování systému Windows Azure**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Zadejte doménu pro klienta služby Windows Azure Active Directory (například contoso.onmicrosoft.com) a klikněte na tlačítko **povolit**:

![](windows-azure-authentication/_static/image3.png)

3. Ve webové ověřování dialogové okno přihlášení jako správce pro vašeho klienta Windows Azure Active Directory:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Povolit okno Azure bez oprávnění správce principem

Pokud nemáte oprávnění globálního správce pro vašeho klienta Windows Azure Active Directory, můžete zrušit zaškrtnutí políčka pro zřizování aplikace.

![](windows-azure-authentication/_static/image6.png)

Dialogové okno se zobrazí **domény**, **Id aplikace hlavní** a **adresa URL odpovědi** které jsou požadované pro zřizování aplikace s Azure Active Directory principem. Budete muset poskytnout tyto informace k někdo, kdo má dostatečná oprávnění ke zřízení aplikace. V tématu[implementaci jednotného přihlašování službou Windows Azure Active Directory - aplikace ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) podrobnosti o tom, jak vytvořit objekt služby ručně pomocí rutiny.  
Po úspěšném zřízení aplikace můžete kliknutím na **pokračovat v aktualizaci souboru web.config s vybraným nastavením**. Pokud chcete pokračovat, vývoj aplikace při čekání na zřizování provést, můžete kliknout na **zavřete na mějte na paměti, nastavení v souboru projektu**. Při příštím vyvolání povolit ověřování systému Windows Azure a zrušte zaškrtnutí políčka zřizování, zobrazí se stejné nastavení a můžete kliknout na **pokračovat**, klepněte na tlačítko **použít tato nastavení v souboru web.config**.

1. Počkejte, dokud vaše aplikace je nakonfigurovaná pro ověřování systému Windows Azure a zřizovat s Windows Azure Active Directory.
2. Jakmile aplikace bylo povoleno ověřování systému Windows Azure, klikněte na možnost **Zavřít:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Stiskněte F5 a spusťte aplikaci. Byste měli získat automaticky přesměrováni na přihlašovací stránku. Používat adresář principem pověření k přihlášení k aplikaci...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Vzhledem k tomu, že vaše aplikace právě používá certifikát podepsaný svým držitelem testovací obdrží upozornění z prohlížeče, který certifikát nebyl vystaven důvěryhodnou certifikační autoritou.

    Toto upozornění můžete ignorovat během vývoje místní kliknutím **pokračovat na tento web:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Jste teď úspěšně přihlásí k aplikaci pomocí ověřování systému Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Povolení služby Windows Azure provádí ověřování následující změny k vaší aplikaci:

- Padělání požadavku křížový proti-Site ([proti útokům CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) – třída ( *aplikace\_Start\AntiXsrfConfig.cs* ) se přidá do projektu.
- Balíčky NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` se přidá do projektu.
- Nastavení Windows Identity Foundation ve vaší aplikaci nakonfiguruje tak, aby přijímal tokeny zabezpečení z klienta služby Windows Azure Active Directory. Kliknutím na bitovou kopii níže zobrazíte rozbalené zobrazení změny provedené *Web.config* souboru.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Objekt služby pro svoji aplikaci v klientovi služby Windows Azure Active Directory se zřídí.
- Je povolený protokol HTTPS.

## <a name="deploy-the-application-to-windows-azure"></a>Nasazení aplikace do systému Windows Azure

Úplné pokyny naleznete v části [nasazení webové aplikace ASP.NET na webu systému Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Publikování aplikace pomocí ověřování systému Windows Azure pro webovou stránku Azure:

1. Klikněte pravým tlačítkem na aplikaci a vyberte **publikovat:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Z tohoto dialogového okna Publikovat Web stáhnout a naimportovat profil publikování pro váš web Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. **Připojení** kartě ukazuje **cílová adresa URL** (veřejné přístupných URL pro aplikaci). Klikněte na tlačítko **ověřit připojení** k testování připojení:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Pokud jste publikovali na tento web Azure dříve zvažte kontrolu **odebrat další soubory v cílovém umístění** nastavení, aby vaše aplikace publikuje ještě jednou. Upozornění **povolit ověřování systému Windows Azure** zaškrtávací políčko je (poštovní úřad).  

    ![](windows-azure-authentication/_static/image10.png)
5. Volitelné: Na **Preview** klikněte na možnost **spustit Náhled** zobrazíte soubory nasazení.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Klikněte na tlačítko **publikovat.**

    Zobrazí se výzva k povolení ověřování systému Windows Azure pro cílového hostitele. Klikněte na tlačítko **povolit** pokračovat:

    ![](windows-azure-authentication/_static/image11.png)
7. Zadejte přihlašovací údaje správce pro vašeho klienta Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Jakmile aplikace byla úspěšně publikována, otevře se prohlížeč k publikované webové stránky.

    > [!NOTE]
    > To může trvat až pět minut (obvykle musí menší) pro vaši aplikaci po povolení ověřování systému Windows Azure pro cílového hostitele s Windows Azure Active Directory kompletní zřízení. Při prvním spuštění aplikace Pokud obdržíte chybu ACS50001: předávající strany s názvem [sféry] nebyl nalezen, počkejte několik minut a zkuste aplikaci znovu spustit.
9. Pokud budete vyzváni, přihlaste se jako uživatel ve vašem adresáři:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Jste teď úspěšně přihlášení do vaší Azure hostované aplikace pomocí ověřování systému Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Známé problémy

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Při použití ověřování systému Windows Azure: < p >< selže ověření na základě role / o: p >

Ověřování systému Windows Azure aktuálně neposkytuje potřebné role deklarací identity tak, že lze provést ověření na základě role. Roli ověřeného uživatele, musíte ručně načíst ze systému Windows Azure Active Directory. < o: p >< / o: p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Procházení k aplikaci pomocí ověřování systému Windows Azure za následek chybu "ACS20016 doméně přihlášeného uživatele (live.com) neodpovídá žádnému povolené domény této služby tokenů zabezpečení" < o: p >< / o: p >

Pokud jste již přihlášeni k Account Microsoft (například hotmail.com, live.com, outlook.com) a pokusíte se o přístup k aplikaci, která povolila ověřování systému Windows Azure může získat odpověď Chyba 400, protože doménu Account Microsoft nebyl rozpoznán balíčkem Windows Azure Active Directory. K přihlášení do aplikace, odhlaste se z Account Microsoft nejprve. < o: p >< / o: p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Protokolování do aplikace pomocí povoleno ověřování systému Windows Azure a X509CertificateValidationMode jiné než žádné má za následek chyby ověření certifikátu pro certifikát accounts.accesscontrol.windows.net < o: p >< / o: p >

Ověření certifikátu není povinný a musí být ponechány zakázáno. Kryptografický otisk certifikátu vystavitele byl ověřen WSFederationAuthenticationModule. < o: p >< / o: p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Při pokusu o povolení ověřování systému Windows Azure webové ověřování dialogové okno zobrazí chyba "ACS20016: doméně přihlášeného uživatele (contoso.onmicrosoft.com) neodpovídá žádné povolené domény této služby tokenů zabezpečení." < o: p >< / o: p >

Tato chyba může zobrazit, když jste dříve úspěšně přihlásí pomocí jiného účtu služby Windows Azure Active Directory z v rámci stejné proces sady Visual Studio. Odhlaste se z zadaný účet nebo restartujte Visual Studio. Pokud jste dříve přihlášení a vybrali možnost "Keep mi přihlášení", pak budete muset vymazat soubory cookie prohlížeče. < o: p >< / o: p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Žádost není platný zprávy protokolu WS-Federation < o: p >< / o: p >

To může nastat, když jste se už přihlásili některé jiné ID společnosti Microsoft na jednu z služeb Azure. Okno prohlížeče použití privátního jako InPrivate v aplikaci Internet Explorer nebo Incognito v prohlížeči Chrome nebo zrušte všechny soubory cookie. <o:p></o:p>

## <a name="additional-resources"></a>Další prostředky

- [Nástroje Microsoft ASP.NET pro systém Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure funkce: Identity](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: vývoj aplikací pro vaši organizaci](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: vývoj aplikací pro několik organizací](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Implementaci jednotného přihlašování pomocí služby Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Jednotné přihlašování se službou Windows Azure Active Directory: podrobné informace](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Používání AD FS 2.0 k implementaci a správě jednotného přihlašování](https://technet.microsoft.com/library/jj205462.aspx)
