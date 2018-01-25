---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: "Jednotné přihlašování (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs"
author: MikeWasson
description: "Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: b3640c94a8ae9ede330c0fe6a392acb5843cb65c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Jednotné přihlašování (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Existuje mnoho problémy se zabezpečením myslet, když vyvíjíte cloudové aplikace, ale pro tuto řadu budete zaměříme na jenom jednom: jednotného přihlašování. Lidé otázku klást, je toto: "I se primárně vytváření aplikací pro zaměstnance Moje společnost; jak hostování těchto aplikací v cloudu a stále povolit, aby uživatelé používali stejný model zabezpečení, Moje zaměstnanci znáte a používat v místním prostředí, pokud jejich používáte systém aplikací, které jsou hostovány v bráně firewall?" Jeden ze způsobů, jak jsme povolit tento scénář se nazývá Azure Active Directory (Azure AD). Azure AD umožňuje zpřístupnit enterprise-obchodní (LOB) aplikacím přes Internet a umožňuje vám umožní provádět tyto aplikace k dispozici také obchodními partnery.

## <a name="introduction-to-azure-ad"></a>Úvod do služby Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) poskytuje [služby Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) v cloudu. Klíčové funkce patří:

- Se integruje s místní služby Active Directory.
- Umožňuje jednotné přihlašování s vaší aplikací.
- Podporuje otevřený standardy, jako [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), a [OAuth 2.0](http://oauth.net/2/).
- Podporuje Enterprise [grafu REST API](https://msdn.microsoft.com/library/hh974476.aspx).

Předpokládejme, že máte místní prostředí Windows Server Active Directory, které používáte k povolení zaměstnanci k přihlášení do intranetu aplikací:

![](single-sign-on/_static/image1.png)

Co Azure AD můžete to udělat, je vytvoření adresáře v cloudu. Je bezplatné funkce a snadno nastavit.

Může být zcela nezávislé z vaší místní služby Active Directory; můžete vložit každý, kdo má v něm a jejich ověření v aplikacích Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Nebo můžete službu integrovat s místní AD.

![AD a službou WAAD integrace](single-sign-on/_static/image3.png)

Nyní všechny zaměstnance, kteří můžou ověřovat na místě, můžete také ověřovat přes Internet – bez nutnosti otevřít brány firewall nebo nasadit všechny nové servery ve vašem datovém centru. Můžete nadále využívat všechny existující služby Active Directory prostředí, které znáte a používají. jednotlivá poskytnout vaše interní aplikace jednotné přihlašování na funkce.

Jakmile jste udělali toto připojení mezi AD a Azure AD, můžete také povolit vašich webových aplikací a mobilních zařízení k ověření zaměstnancům v cloudu a můžete povolit aplikacím třetích stran, jako je například aplikace Office 365 a SalesForce.com, Google, tak, aby přijímal vaší přihlašovací údaje zaměstnanců. Pokud používáte Office 365, již nastavení s Azure AD protože Office 365 používá Azure AD pro ověřování a autorizaci.

![3. stran aplikace](single-sign-on/_static/image4.png)

Výhodou tohoto přístupu je, že kdykoli organizaci přidá nebo odstraní uživatele, nebo uživatel změní heslo, použijte stejný proces, který používáte dnes v místním prostředí. Všechny z místní AD změny rozšířeny automaticky cloudového prostředí.

Pokud vaše společnost používá nebo přesun do služeb Office 365 je dobrá zpráva, že budete mít Azure AD nastavit automaticky protože Office 365 používá Azure AD pro ověřování. Abyste mohli snadno používat ve svých vlastních aplikacích stejné ověřování, který používá Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Nastavení klienta služby Azure AD

adresář služby Azure AD se nazývá na Azure AD [klienta](https://technet.microsoft.com/library/jj573650.aspx), a nastavení klienta je velmi snadné. Ukážeme vám jak se provádí v portálu pro správu Azure k objasnění konceptů, ale samozřejmě jako dalších funkcí portálu můžete provést také ho pomocí skriptu nebo rozhraní API pro správu.

V portálu pro správu klikněte na kartu služby Active Directory.

![Službou WAAD portálu](single-sign-on/_static/image5.png)

Máte jednoho klienta Azure AD automaticky k účtu Azure, a můžete kliknout na **přidat** tlačítko v dolní části stránky a vytvářet další adresáře. Měli byste jeden pro testovací prostředí a jeden pro produkční prostředí, například. Vezměte v úvahu pečlivě co zadáte název nového adresáře. Pokud použijete název adresáře a pak použijete název znovu pro uživatele, který může být matoucí.

![Přidejte adresář](single-sign-on/_static/image6.png)

Portál obsahuje plnou podporu pro vytváření, odstraňování a Správa uživatelů v tomto prostředí. Například pokud chcete přidat uživatele přejděte na **uživatelé** a klikněte **přidat uživatele** tlačítko.

![Tlačítko Přidat uživatele](single-sign-on/_static/image7.png)

![Přidat uživatele – dialogové okno](single-sign-on/_static/image8.png)

Můžete vytvořit nového uživatele, který se nachází pouze v tomto adresáři nebo Account Microsoft můžete zaregistrovat jako uživatel v tomto adresáři nebo zaregistrovat nebo uživatele z jiného adresáře služby Azure AD jako uživatel v tomto adresáři. (V skutečné adresáři, jako výchozí doménu bude ContosoTest.onmicrosoft.com. Můžete také použít doménu podle vlastní volby, například contoso.com.)

![Typy uživatelů](single-sign-on/_static/image9.png)

![Přidat uživatele – dialogové okno](single-sign-on/_static/image10.png)

Můžete přiřadit uživatele k roli.

![Profil uživatele](single-sign-on/_static/image11.png)

A účet je vytvořen s dočasným heslem.

![Dočasné heslo](single-sign-on/_static/image12.png)

Uživatelé, vytvoříte tímto způsobem okamžitě přihlásit vašich webových aplikací pomocí tohoto cloudu.

Co je výborně hodí pro podnikové jednotné přihlašování, ale je **integrace adresáře** karty:

![Karta integrace adresáře](single-sign-on/_static/image13.png)

Pokud povolíte integraci adresáře a [stáhněte si nástroj](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), budete moct synchronizovat tento adresář cloudu s vaší stávající místní služby Active Directory, kterou používáte už ve vaší organizaci. Potom všechny uživatele uložené v adresáři se zobrazí v tomto adresáři cloudu. Cloudové aplikace, můžete nyní ověřovat všechny zaměstnance pomocí přihlašovacích existující služby Active Directory. A všechny, to je volné – nástroj pro synchronizaci i Azure AD, sám sebe.

Tento nástroj je průvodce, který se snadno používá, jak je vidět na tyto snímky obrazovky. Tyto nejsou úplné pokyny, jenom jako příklad ukazuje základní proces. Podrobné informace how-k-proveďte it, najdete v článku odkazy v [prostředky](#resources) části na konci kapitoly.

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image14.png)

Klikněte na tlačítko **Další**a pak zadejte svoje přihlašovací údaje služby Azure Active Directory.

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image15.png)

Klikněte na tlačítko **Další**a pak zadejte místní přihlašovací údaje služby AD.

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image16.png)

Klikněte na tlačítko **Další**a pak vyberte, pokud chcete ukládání klíče hash hesla služby AD v cloudu.

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image17.png)

Hodnota hash hesla, který chcete uložit v cloudu je jednosměrná hodnota hash; skutečných hesel nikdy ukládají ve službě Azure AD. Pokud se rozhodnete pro ukládání hodnot hash v cloudu, budete muset použít [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Existují také [dalších faktorů, které je třeba zvážit při výběru, zda se má používat služba AD FS](https://technet.microsoft.com/library/jj573653.aspx). Možnosti služby AD FS vyžaduje několik další konfigurační kroky.

Pokud se rozhodnete ukládat hodnoty hash v cloudu, s tím budete hotovi a spustí nástroj pro synchronizaci adresářů, po kliknutí na tlačítko **Další**.

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image18.png)

A za pár minut jste hotovi.

![Průvodce konfigurací nástroje synchronizace službou WAAD](single-sign-on/_static/image19.png)

Potřebujete ji spustit na jednom řadiči domény v organizaci, v systému Windows 2003 nebo vyšší. A není nutné restartovat. Když jste hotovi, všichni uživatelé jsou v cloudu a můžete provést jednotné přihlašování v jakémkoli web nebo mobilní aplikace, pomocí SAML, WS-Fed nebo OAuth.

Někdy jsme získat vyzváni, o tom, jak zabezpečené jde – Microsoft používá ho pro své vlastní citlivých obchodních dat? A odpovíte kladně, které jsme provést. Například, pokud přejdete na interní web Microsoft SharePoint na [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), získat zobrazí výzva k přihlášení.

![Přihlášení k Office 365](single-sign-on/_static/image20.png)

Microsoft má povolenou službou AD FS, takže když zadáte ID společnosti Microsoft, budete přesměrováni na přihlašovací stránku služby AD FS.

![Přihlášení služby AD FS](single-sign-on/_static/image21.png)

A až zadáte přihlašovací údaje uložené v interní účet Microsoft AD, budete mít přístup k této interní aplikace.

![Server MS SharePoint](single-sign-on/_static/image22.png)

Používáme přihlášení serveru služby AD, především, protože jsme už měli ADFS nastavit před Azure AD jsou k dispozici, ale proces přihlášení prochází přes adresář služby Azure AD v cloudu. Nemůžeme vložit naše důležité dokumenty, Správa zdrojového kódu, soubory správy výkonu, prodejní sestavy a další v cloudu a používají k zabezpečení je toto přesně stejnou řešení.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Vytvořit aplikaci ASP.NET, která používá Azure AD pro jednotné přihlašování

Visual Studio skutečně usnadňuje vytvořit aplikaci, která používá Azure AD pro jednotné přihlašování, jak je vidět z několika snímky obrazovky.

Když vytvoříte novou aplikaci ASP.NET, MVC ani webové formuláře, je výchozí metodou ověřování ASP.NET Identity. Pokud chcete změnit, do Azure AD, kliknutí na tlačítko **změnit ověřování** tlačítko.

![Změna ověřování](single-sign-on/_static/image23.png)

Vyberte účty organizace, zadejte název domény a pak vybrat jednotné přihlašování.

![Konfigurace ověřování dialogové okno](single-sign-on/_static/image24.png)

Můžete také číst aplikace nebo pro čtení a zápis oprávnění pro adresář data. Pokud tak učiníte, můžete použít [REST API služby Azure Graph](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) k vyhledání uživatelů telefonní číslo, zjistěte, pokud jsou v kanceláři, při posledním přihlášení na atd.

Který je všechno, co musíte udělat – Visual Studio vyzve k zadání pověření pro správce pro vašeho tenanta Azure AD, a poté konfiguruje váš projekt a klientovi Azure AD pro novou aplikaci.

Při spuštění projektu, se zobrazí na přihlašovací stránku a můžete se přihlásit pomocí přihlašovacích údajů uživatele v adresáři služby Azure AD.

![Přihlášení účtu organizace](single-sign-on/_static/image25.png)

![Přihlášení](single-sign-on/_static/image26.png)

Když nasadíte aplikaci do Azure, stačí je vyberte **povolit ověřování organizace** zaškrtněte políčko a ještě jednou Visual Studio postará všechny konfigurace pro vás.

![Průvodci Publikovat Web](single-sign-on/_static/image27.png)

Tyto snímky obrazovky pocházet z úplný podrobný návod, jak sestavit aplikaci, která používá ověřování Azure AD: [vývoj aplikace ASP.NET se službou Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Souhrn

V této kapitole jste viděli, že Azure Active Directory, Visual Studio a technologii ASP.NET, můžete snadno nastavit jednotné přihlašování v internetových aplikací pro uživatele ve vaší organizaci. Vaši uživatelé přihlásit Internetu aplikací pomocí stejných přihlašovacích údajů, které používají k přihlášení pomocí služby Active Directory v interní síti.

[Další kapitoly](data-storage-options.md) zjistí možnosti úložiště dat, která je k dispozici pro cloudové aplikace.

<a id="resources"></a>
## <a name="resources"></a>Prostředky

Další informace naleznete v následujících materiálech:

- [Dokumentaci ke službě Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Stránky portálu pro Azure AD dokumentaci na webu windowsazure.com. Podrobné pokyny krok za krokem, najdete v článku **vývoj** části.
- [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Stránky portálu pro dokumentaci o službě Multi-Factor authentication v Azure.
- [Možnosti ověřování účtu organizace](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Vysvětlení možností ověřování Azure AD v dialogu Nový projekt Visual Studio 2013.
- [Microsoft Patterns and Practices - federované Identity vzor](https://msdn.microsoft.com/library/dn589790.aspx).
- [Postupy: Instalace nástroje Azure Active Directory Sync](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 mapa obsahu](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Odkazy na dokumentaci o služby AD FS 2.0.
- [Ověřování na základě rolí a na základě seznamu ACL v aplikaci Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Ukázkovou aplikaci.
- [Blog Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).
- [Řízení přístupu v modelu BYOD a integrace adresáře v hybridní infrastrukturu identit](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Technická Ed 2014 relace video Gayana Bagdasaryan.

>[!div class="step-by-step"]
[Předchozí](web-development-best-practices.md)
[další](data-storage-options.md)
