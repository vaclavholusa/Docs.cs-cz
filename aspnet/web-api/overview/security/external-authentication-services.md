---
uid: web-api/overview/security/external-authentication-services
title: "Externí ověřovací služby s rozhraním ASP.NET Web API (C#) | Microsoft Docs"
author: rmcmurray
description: "Popisuje použití externí služby ověřování v rozhraní ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 5d6e6727f387d047e7b41a6efa0d2dadf467558e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>Externí ověřovací služby s rozhraním ASP.NET Web API (C#)
====================
podle [Roberta Mcmurrayho](https://github.com/rmcmurray)

Visual Studio 2013 a ASP.NET 4.5.1 rozšířit možnosti zabezpečení pro [jednostránkové aplikace](../../../single-page-application/index.md) (SPA) a [webového rozhraní API](../../index.md) služby pro integraci s externí ověřovací služby, které zahrnují několik Účtu OAuth nebo OpenID a ověřování služby sociálních médií: Accounts Microsoft, Twitter, Facebook či Google.

### <a name="in-this-walkthrough"></a>V tomto návodu

- [Pomocí externí ověřovací služby](#USING)
- [Vytvoření ukázkové webové aplikace](#SAMPLE)
- [Povolení ověřování Facebook](#FACEBOOK)
- [Povolení ověřování Google](#GOOGLE)
- [Povolení ověřování Microsoft](#MICROSOFT)
- [Povolení ověřování služby Twitter.](#TWITTER)
- [Další informace](#MOREINFO)

    - [Kombinování externí ověřovací služby](#COMBINE)
    - [Konfigurace služby IIS Express použít plně kvalifikovaný název domény](#FQDN)
    - [Jak získat nastavení aplikace pro ověřování Microsoft](#OBTAIN)
    - [Volitelné: Zakázat místní registraci](#DISABLE)

### <a name="prerequisites"></a>Požadavky

Následují příklady v tomto návodu, musíte mít následující:

- Visual Studio 2013
- Účet pro minimálně jeden z následujících služeb externího ověřování:

    - Uživatelský účet Google
    - Účet pro vývojáře se identifikátor aplikace a tajný klíč pro jednu z následujících ověřování služby sociálních médií:

        - Účty Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Pomocí externí ověřovací služby

Celou řadu externí ověřovací služby, které jsou aktuálně k dispozici Nápověda na webu vývojáři ke snížení vývoj čas při vytváření nové webové aplikace. Webové obvykle mají uživatelé několik existujících účtů pro oblíbené webové služby a webů sociálních médií, proto při implementuje aplikace webové ověřování služby z externí webovou službu nebo webu sociálních médií, ukládá okamžiku vývoje by vznikly vytváření implementace ověřování. Použití služeb externího ověřování uloží koncoví uživatelé nebudou muset vytvořit jiný účet pro webové aplikace a také z si museli pamatovat jiné uživatelské jméno a heslo.

V minulosti, vývojáři máte dvě možnosti: vytvoření vlastní implementace ověřování, nebo zjistěte, jak integrovat do svých aplikací služby externího ověřování. Na nejzákladnější úrovni následující diagram znázorňuje toku jednoduché požadavků pro uživatelského agenta (webový prohlížeč), který vyžaduje informace z webové aplikace, který je nakonfigurován pro použití služby externí ověřování:

[![](external-authentication-services/_static/image2.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image1.png)

Na předchozím obrázku uživatelský agent (nebo webový prohlížeč v tomto příkladu) odešle požadavek na webovou aplikaci, která přesměruje webový prohlížeč na služby externího ověřování. Uživatelský agent odešle přihlašovací údaje ke službě externího ověřování, a pokud uživatelský agent byl úspěšně ověřen, externí ověřovací služby přesměruje uživatelského agenta na původní webové aplikace s určitou formu token, který uživatelský agent odešle do webové aplikace. Webové aplikace použije token k ověření, že uživatelský agent byl úspěšně ověřen službou externího ověřování a webové aplikace může využívat token získat další informace o uživatelský agent. Jakmile aplikace dokončí zpracování informací uživatelského agenta, webové aplikace vrátí odpovídající odpověď pro uživatelského agenta na základě nastavení autorizace.

V tomto příkladu druhý uživatelský agent vyjedná s webovou aplikací a serveru externího ověřování a webové aplikace provede další komunikace se serverem externí autorizace pro načtení Další informace o uživateli Agent:

[![](external-authentication-services/_static/image4.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image3.png)

Visual Studio 2013 a ASP.NET 4.5.1 usnadnění integrace s externí ověřovací služby pro vývojáře díky integraci u následujících služeb ověřování:

- Facebook
- Google
- Microsoft Accounts (účty Windows Live ID)
- Služby Twitter.

V příkladech v tomto návodu se ukazují, jak nakonfigurovat každou z podporovaných externí ověřovací služby pomocí nové šablony webové aplikace ASP.NET, který se dodává s Visual Studio 2013.

> [!NOTE]
> V případě potřeby můžete přidat váš plně kvalifikovaný název domény do nastavení služby externího ověřování. Tento požadavek je založena na omezení zabezpečení u některých služeb externího ověřování, které vyžadují úplný název domény v nastavení aplikace tak, aby odpovídala plně kvalifikovaný název domény, který je používán vašim klientům. (Kroky pro to bude značně lišit pro každou službu externího ověřování, budete muset najdete v dokumentaci pro každou službu externího ověřování chcete zobrazit, pokud to je potřeba a konfiguraci těchto nastavení.) Pokud potřebujete konfiguraci služby IIS Express použít plně kvalifikovaný název domény pro testování toto prostředí, najdete v článku [konfigurace služby IIS Express použít plně kvalifikovaný název domény](#FQDN) dále v tomto návodu.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Vytvoření ukázkové webové aplikace

Následující postup vás provede vytvoření ukázkové aplikace pomocí šablony webové aplikace ASP.NET a tato ukázková aplikace bude používat pro každou z externí ověřovací služby dále v tomto návodu.

Spusťte Visual Studio 2013 vyberte **nový projekt** z úvodní stránky. Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.

[![](external-authentication-services/_static/image6.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image5.png)

Když **nový projekt** se zobrazí dialogové okno, vyberte **nainstalovaná** **šablony** a rozbalte **Visual C#**. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET**. Zadejte název projektu a klikněte na tlačítko **OK**.

[![](external-authentication-services/_static/image8.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image7.png)

Když **nový projekt ASP.NET** jsou zobrazeny, vyberte **SPA** šablonu a klikněte na tlačítko **vytvořit projekt**.

[![](external-authentication-services/_static/image10.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image9.png)

Počkejte jako Visual Studio 2013 vytvoří projektu.

[![](external-authentication-services/_static/image12.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image11.png)

Po dokončení vytvoření projektu Visual Studio 2013 otevřete *Startup.Auth.cs* soubor, který se nachází v **aplikace\_spustit** složky.

[![](external-authentication-services/_static/image14.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image13.png)

Při prvním vytvoření projektu, žádné externí ověřovací služby jsou povoleny v *Startup.Auth.cs* souboru; následující obrázek znázorňuje, co může vypadat kódu, s oddíly zvýrazněná o tom, kde byste měli povolit služby externího ověřování a relevantní nastavení, abyste mohli používat ověřování Accounts Microsoft, Twitteru, Facebooku a Google s vaší aplikace ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Po stisknutí klávesy F5 můžete vytvářet a ladit webové aplikace se zobrazí přihlašovací obrazovku, kde uvidíte, že nebyly definovány žádné externí ověřovací služby.

[![](external-authentication-services/_static/image16.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image15.png)

V následujících částech se dozvíte, jak můžete povolit každou z externí ověřovací služby, které jsou k dispozici s technologií ASP.NET v sadě Visual Studio 2013.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Povolení ověřování Facebook

Pomocí služby Facebook ověřování vyžaduje, abyste k vytvoření účtu vývojáře Facebook a projekt bude vyžadovat ID aplikace a tajný klíč ze sítě Facebook, chcete-li funkce. Informace o vytvoření účtu vývojáře Facebook a získání ID aplikace a tajný klíč najdete v tématu [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Po získání ID aplikace a tajný klíč, použijte následující postup povolení ověřování Facebook pro webové aplikace:

1. Pokud projekt je otevřen v sadě Visual Studio 2013, otevřete *Startup.Auth.cs* souboru:

    [![](external-authentication-services/_static/image18.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image17.png)
2. Vyhledejte části zvýrazněný kód:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Odeberte &quot; // &quot; znaky, zrušte komentář u zvýrazněné řádky kódu, a poté přidejte ID aplikace a tajný klíč. Jakmile přidáte tyto parametry, můžete znovu zkompiluje projektu:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Po stisknutí klávesy F5 otevřete webovou aplikaci ve webovém prohlížeči, zobrazí se, že Facebook byla definována jako služba externího ověřování:

    [![](external-authentication-services/_static/image20.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image19.png)
5. Když kliknete **Facebook** tlačítko Prohlížeč bude přesměrován na přihlašovací stránku služby Facebook:

    [![](external-authentication-services/_static/image22.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image21.png)
6. Po zadání přihlašovacích údajů Facebook a klikněte na tlačítko **přihlásit**, webový prohlížeč, bude přesměrován zpět do webové aplikace, který zobrazí výzvu k **uživatelské jméno** , kterou chcete přidružit k vaší Účet Facebook:

    [![](external-authentication-services/_static/image24.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image23.png)
7. Po zadání uživatelského jména a kliknutí na **zaregistrovat** tlačítko webové aplikace se zobrazí výchozí **domovskou stránku** pro váš účet Facebook:

    [![](external-authentication-services/_static/image26.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Povolení ověřování Google

Je mnohem nejjednodušší služeb externího ověřování povolit, protože nevyžaduje účet pro vývojáře, ani nevyžaduje další informace, jako je ID aplikace nebo tajný klíč jako externí ověřovací služby Google vyžadují.

Pokud chcete povolit ověřování Google pro vaši webovou aplikaci, použijte následující kroky:

1. Pokud projekt je otevřen v sadě Visual Studio 2013, otevřete *Startup.Auth.cs* souboru:

    [![](external-authentication-services/_static/image28.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image27.png)
2. Vyhledejte části zvýrazněný kód:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Odeberte &quot; // &quot; znaky, zrušte komentář u zvýrazněný řádek kódu, a pak znovu zkompiluje projektu:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Po stisknutí klávesy F5 otevřete webovou aplikaci ve webovém prohlížeči, zobrazí se, že Google byla definována jako služba externího ověřování:

    [![](external-authentication-services/_static/image30.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image29.png)
5. Když kliknete **Google** tlačítko Prohlížeč bude přesměrován na přihlašovací stránku Google:

    [![](external-authentication-services/_static/image32.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image31.png)
6. Po zadání přihlašovacích údajů Google a klikněte na tlačítko **přihlášení**, Google vás vyzve k ověření, že vaše webová aplikace má oprávnění pro přístup k účtu Google:

    [![](external-authentication-services/_static/image34.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image33.png)
7. Když kliknete na tlačítko **přijmout**, webový prohlížeč, bude přesměrován zpět na webovou aplikaci, která vás vyzve **uživatelské jméno** , kterou chcete přidružit svůj účet Google:

    [![](external-authentication-services/_static/image36.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image35.png)
8. Po zadání uživatelského jména a kliknutí na **zaregistrovat** tlačítko webové aplikace se zobrazí výchozí **domovskou stránku** pro váš účet Google:

    [![](external-authentication-services/_static/image38.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Povolení ověřování Microsoft

Ověřování Microsoft vyžaduje, abyste vytvořili účet pro vývojáře a jeho vyžaduje ID klienta a tajný klíč klienta fungovat. Informace o vytvoření účtu Microsoft developer a získání ID klienta a tajný klíč klienta najdete v tématu [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).

Po získání uživatelský klíč a uživatelský tajný klíč, použijte následující postup povolení ověřování Microsoft pro webové aplikace:

1. Pokud projekt je otevřen v sadě Visual Studio 2013, otevřete *Startup.Auth.cs* souboru:

    [![](external-authentication-services/_static/image40.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image39.png)
2. Vyhledejte části zvýrazněný kód:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Odeberte &quot; // &quot; znaky, zrušte komentář u zvýrazněné řádky kódu, a poté přidejte ID klienta a tajný klíč klienta. Jakmile přidáte tyto parametry, můžete znovu zkompiluje projektu:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Po stisknutí klávesy F5 otevřete webovou aplikaci ve webovém prohlížeči, zobrazí se, že společnosti Microsoft byla definována jako služba externího ověřování:

    [![](external-authentication-services/_static/image42.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image41.png)
5. Když kliknete **Microsoft** tlačítko Prohlížeč bude přesměrován na přihlašovací stránku společnosti Microsoft:

    [![](external-authentication-services/_static/image44.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image43.png)
6. Po zadání přihlašovacích údajů Microsoft a klikněte na tlačítko **přihlášení**, zobrazí se výzva k ověření, že vaše webová aplikace má oprávnění pro přístup k účtu Microsoft:

    [![](external-authentication-services/_static/image46.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image45.png)
7. Když kliknete na tlačítko **Ano**, webový prohlížeč, bude přesměrován zpět na webovou aplikaci, která vás vyzve **uživatelské jméno** , kterou chcete přidružit svůj účet Microsoft:

    [![](external-authentication-services/_static/image48.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image47.png)
8. Po zadání uživatelského jména a kliknutí na **zaregistrovat** tlačítko webové aplikace se zobrazí výchozí **domovskou stránku** pro svůj účet Microsoft:

    [![](external-authentication-services/_static/image50.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Povolení ověřování služby Twitter.

Twitter ověřování vyžaduje, abyste vytvořili účet pro vývojáře a chcete-li funkce vyžaduje uživatelský klíč a uživatelský tajný klíč. Informace o vytváření vývojářského účtu služby Twitter a získat uživatelský klíč a uživatelským utajením najdete v tématu [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Po získání uživatelský klíč a uživatelský tajný klíč, použijte následující postup povolení ověřování služby Twitter pro webové aplikace:

1. Pokud projekt je otevřen v sadě Visual Studio 2013, otevřete *Startup.Auth.cs* souboru:

    [![](external-authentication-services/_static/image52.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image51.png)
2. Vyhledejte části zvýrazněný kód:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Odeberte &quot; // &quot; znaky, zrušte komentář u zvýrazněné řádky kódu, a poté přidejte uživatelský klíč a uživatelský tajný klíč. Jakmile přidáte tyto parametry, můžete znovu zkompiluje projektu:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Po stisknutí klávesy F5 otevřete webovou aplikaci ve webovém prohlížeči, zobrazí se, že Twitter byla definována jako služba externího ověřování:

    [![](external-authentication-services/_static/image54.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image53.png)
5. Když kliknete **Twitter** tlačítko Prohlížeč bude přesměrován na přihlašovací stránku služby Twitter:

    [![](external-authentication-services/_static/image56.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image55.png)
6. Po zadání přihlašovacích údajů služby Twitter a klikněte na tlačítko **autorizovat aplikaci**, webový prohlížeč, bude přesměrován zpět na webovou aplikaci, která vás vyzve **uživatelské jméno** , kterou chcete přidružit k váš účet služby Twitter:

    [![](external-authentication-services/_static/image58.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image57.png)
7. Po zadání uživatelského jména a kliknutí na **zaregistrovat** tlačítko webové aplikace se zobrazí výchozí **domovskou stránku** pro váš účet služby Twitter:

    [![](external-authentication-services/_static/image60.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Další informace

Další informace o vytváření aplikací, které používají OAuth a OpenID viz následující adresy URL:

- [https://go.microsoft.com/fwlink/?LinkId=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkId=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Kombinování externí ověřovací služby

Pro větší flexibilitu můžete definovat více služeb externího ověřování ve stejnou dobu – to umožňuje vaší webové aplikace uživatelům použít účet ze všech povoleno externí ověřovací služby:

[![](external-authentication-services/_static/image62.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Konfigurace služby IIS Express použít plně kvalifikovaný název domény

Někteří poskytovatelé externího ověřování nepodporují testování vaší aplikace pomocí adresy HTTP jako `http://localhost:port/`. Chcete-li tento problém obejít, můžete přidat statických mapování plně kvalifikovaný název domény (FQDN) do souboru HOSTITELŮ a nakonfigurovat možnosti projektu v sadě Visual Studio 2013 použít plně kvalifikovaný název domény pro testování ladění. Uděláte to tak, použijte následující postup:

- Přidejte statické plně kvalifikovaný název domény mapování souboru hostitele:

    1. Otevřete příkazový řádek se zvýšenými oprávněními v systému Windows.
    2. Zadejte následující příkaz:

        <kbd>%WinDir%\system32\drivers\etc\hosts Poznámkový blok</kbd>
    3. Přidejte do souboru HOSTITELŮ záznam takto:

        <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
    4. Uložte a zavřete soubor HOSTITELŮ.
- Konfigurace projektu Visual Studio pro použití plně kvalifikovaný název domény:

    1. Pokud váš projekt otevřete v sadě Visual Studio 2013, klikněte na **projektu** nabídce a pak vyberte vlastnosti projektu. Například můžete vybrat **WebApplication1 vlastnosti**.
    2. Vyberte **webové** kartě.
    3. Zadejte plně kvalifikovaný název vaší domény pro **projektu Url**. Například zadejte <kbd>http://www.wingtiptoys.com</kbd> Pokud, který byl mapování plně kvalifikovaný název domény, který jste přidali do souboru HOSTITELŮ.
- Konfigurace služby IIS Express použít plně kvalifikovaný název domény pro vaši aplikaci:

    1. Otevřete příkazový řádek se zvýšenými oprávněními v systému Windows.
    2. Zadejte následující příkaz a změňte do složky služby IIS Express:

        <kbd>CD /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Zadejte následující příkaz pro přidání do aplikace plně kvalifikovaný název domény:

        <kbd>Appcmd.exe nastavení konfigurace-section:system.applicationHost/sites / +&quot;[name = 'WebApplication1'] .bindings. [protokol http, bindingInformation = ='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

 Kde **WebApplication1** je název projektu a **bindingInformation** obsahuje plně kvalifikovaný název domény a číslo portu, které chcete použít pro testování.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Jak získat nastavení aplikace pro ověřování Microsoft

Propojení aplikace pro Windows Live pro Microsoft Authentication je jednoduchý proces. Pokud už nejsou propojené aplikace pro Windows Live, můžete použít následující kroky:

1. Přejděte do [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) a zadejte název účtu Microsoft a heslo, když se zobrazí výzva, a pak klikněte na tlačítko **přihlášení**:

    [![](external-authentication-services/_static/image64.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image63.png)
2. Zadejte název a jazyk aplikace po zobrazení výzvy a pak klikněte na tlačítko **souhlasím**:

    [![](external-authentication-services/_static/image66.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image65.png)
3. Na **nastavení rozhraní API** stránky pro vaši aplikaci, zadejte přesměrování doménu pro vaše aplikace a zkopírujte **ID klienta** a **tajný klíč klienta** pro váš projekt a potom Klikněte na tlačítko **Uložit**:

    [![](external-authentication-services/_static/image68.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Volitelné: Zakázat místní registraci

Aktuální místní registraci funkce ASP.NET nezabrání automatizovaným programům (robotů) člena vytváření účtů; například pomocí technologie robota prevence a ověřování, jako je [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). Z tohoto důvodu byste měli odebrat místní přihlašovací formulář a registrace odkaz na přihlašovací stránku. Chcete-li to provést, otevřete  *\_Login.cshtml* stránky ve vašem projektu a pak komentář řádků pro panel místní přihlášení a odkaz na registraci. Výsledná stránka by měla jako jako následující ukázka kódu:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Po panelu Místní přihlášení a odkaz na registraci byly zakázány, vaší přihlašovací stránce se zobrazí pouze zprostředkovatele externího ověřování, které jste povolili:

[![](external-authentication-services/_static/image70.png "Kliknutím rozbalíte bitovou kopii")](external-authentication-services/_static/image69.png)
