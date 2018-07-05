---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Jednotné přihlašování (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 213b8fe091bcac7f55fd62ab305c77fcbc5a77ad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374377"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Jednotné přihlašování (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Existuje mnoho problémů se zabezpečením zamyslet, když vytváříte cloudové aplikace, ale pro tuto sérii zaměříme na jenom jednom: jednotné přihlašování. Lidé otázku se často ptají, je to: "jsem jsem primárně vytváření aplikací pro zaměstnance společnosti; jak hostování těchto aplikací v cloudu a stále povolit je, aby používaly stejný model zabezpečení, který svých zaměstnanců znají a používat v místním prostředí, když běží aplikace, které jsou hostované za firewallem?" Jeden ze způsobů, jak můžeme povolit tento scénář se nazývá Azure Active Directory (Azure AD). Azure AD umožňuje zpřístupnit enterprise-obchodní (LOB) aplikacím přes Internet a umožňuje zpřístupnit tyto aplikace a obchodními partnery.

## <a name="introduction-to-azure-ad"></a>Úvod do služby Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) poskytuje [služby Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) v cloudu. Klíčové funkce patří:

- Integruje místní služby Active Directory.
- Umožňuje jednotné přihlašování s vašimi aplikacemi.
- Podporuje otevřených standardů, jako [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), a [OAuth 2.0](http://oauth.net/2/).
- Podporuje organizace [rozhraní REST API služby Graph](https://msdn.microsoft.com/library/hh974476.aspx).

Předpokládejme, že máte v místním prostředí systému Windows Server Active Directory, který používáte, abyste mohli zaměstnancům zajistit Přihlaste se k intranetu aplikace:

![](single-sign-on/_static/image1.png)

Co Azure AD díky tomu můžete je vytvořit adresář v cloudu. Je to bezplatná funkce a umožňují snadné nastavení.

Může být zcela nezávislý na vaše místní službu Active Directory. můžete vložit kdokoli má v sobě a jejich ověření v aplikacích Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Nebo ji integrovat s místní AD.

![AD a WAAD integrace](single-sign-on/_static/image3.png)

Nyní všichni zaměstnanci, kteří můžou ověřovat místní objekt moci ověřovat také přes Internet – aniž by bylo nutné otevírat brány firewall nebo nasazovat nové servery ve vašem datovém centru. Můžete dál využívat všechny stávajícího prostředí Active Directory, které znáte a používáte poskytnout vaše interní aplikace jednotného přihlašování na funkce.

Po provedení tohoto připojení mezi službami AD a Azure AD, můžete také povolit webových aplikací a mobilních zařízení k ověření vaši zaměstnanci v cloudu a můžete povolit aplikacím třetích stran, jako je Office 365 a SalesForce.com, Google apps, tak, aby přijímal vaší přihlašovací údaje zaměstnanců. Pokud používáte Office 365, máte již nastavíte s Azure AD vzhledem k tomu Office 365 používá Azure AD pro ověřování a autorizaci.

![3. stran aplikace](single-sign-on/_static/image4.png)

Výhodou tohoto přístupu je, že vaše organizace přidá nebo odstraní uživatele, nebo uživatel změní heslo, použijte stejný postup, který už dnes používáte v místním prostředí. Všechny z místní AD změny automaticky rozšíří na cloudovém prostředí.

Pokud vaše společnost používá nebo přesunutí do Office 365, dobrou zprávou je, že bude nutné nastavit automaticky protože Office 365 používá Azure AD pro ověřování Azure AD. Abyste mohli snadno použít ve své vlastní aplikace stejným ověřování, které používá Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Nastavení tenanta služby Azure AD

adresář Azure AD se volá s Azure AD [tenanta](https://technet.microsoft.com/library/jj573650.aspx), a nastavení klienta je poměrně snadné. Vám ukážeme, jak se provádí v portálu pro správu Azure k objasnění konceptů, ale samozřejmě jako dalších funkcí portálu můžete provést také ho pomocí skriptu nebo rozhraní API pro správu.

V portálu pro správu klikněte na kartu služby Active Directory.

![WAAD portálu](single-sign-on/_static/image5.png)

Máte jednoho tenanta Azure AD automaticky ke svému účtu Azure a můžete kliknout **přidat** tlačítko v dolní části stránky a vytvořte další adresáře souborů. Jednu pro testovací prostředí a druhý pro produkční prostředí, může být například vhodné. Pečlivě zvážit, co zadáte název nového adresáře. Pokud pak použijete název znovu jednoho z uživatelů, použijte název vašeho adresáře, který může být matoucí.

![Přidat adresář](single-sign-on/_static/image6.png)

Portál obsahuje plnou podporu pro vytváření, odstraňování a správu uživatelů v rámci tohoto prostředí. Například, chcete-li přidat uživatele přejít na **uživatelé** kartě a klikněte na tlačítko **přidat uživatele** tlačítko.

![Tlačítko Přidat uživatele](single-sign-on/_static/image7.png)

![Přidat dialog uživatele](single-sign-on/_static/image8.png)

Můžete vytvořit nového uživatele, který se nachází pouze v tomto adresáři, nebo můžete zaregistrovat Account Microsoft jako uživatel v tomto adresáři, nebo register nebo uživatele z jiného adresáře služby Azure AD jako uživatel v tomto adresáři. (V adresáři skutečné, výchozí domény by ContosoTest.onmicrosoft.com. Můžete také použít doménu podle vlastního výběru, třeba contoso.com.)

![Uživatelské typy](single-sign-on/_static/image9.png)

![Přidat dialog uživatele](single-sign-on/_static/image10.png)

Přiřadit uživatele k roli.

![Profil uživatele](single-sign-on/_static/image11.png)

A účet se vytvoří s dočasným heslem.

![Dočasné heslo](single-sign-on/_static/image12.png)

Uživatele vytvoříte tímto způsobem můžete okamžitě Přihlaste se k vaší webové aplikace s využitím tento adresář v cloudu.

Co se skvěle hodí pro podnikové jednotné přihlašování, ale je **integrace adresáře** kartu:

![Karta integrace adresáře](single-sign-on/_static/image13.png)

Pokud povolíte integrace adresáře a [stáhněte si nástroj](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), můžete synchronizovat tento adresář v cloudu s vaší stávající místní služby Active Directory, který již používáte ve vaší organizaci. Potom všechny uživatele uložené v adresáři se zobrazí tento adresář v cloudu. Cloudové aplikace může ověřit nyní všechny zaměstnance pomocí existujících přihlašovacích údajů služby Active Directory. A všechno, co to je bezplatné – nástroj pro synchronizaci i samotné služby Azure AD.

Tento nástroj je průvodce, který se snadno používá, jak je vidět z těchto obrazovek. Ty nejsou úplné pokyny uvedené jenom jako příklad zobrazuje základní proces. Podrobnější postupy-k-it, najdete na odkazech v [prostředky](#resources) části na konci kapitoly.

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image14.png)

Klikněte na tlačítko **Další**a pak zadejte svoje přihlašovací údaje Azure Active Directory.

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image15.png)

Klikněte na tlačítko **Další**a poté zadejte místní přihlašovací údaje služby AD.

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image16.png)

Klikněte na tlačítko **Další**a pak poznáte, zda chcete ukládat hodnoty hash hesla služby AD v cloudu.

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image17.png)

Hodnota hash hesla, které lze uložit do cloudu tvoří otisk; skutečná hesla se nikdy neukládají v Azure AD. Pokud se rozhodnete pro ukládání hodnoty hash v cloudu, budete muset použít [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (služby AD FS). Existují také [dalších faktorů, které je třeba zvážit při výběru, jestli se mají použít služby AD FS](https://technet.microsoft.com/library/jj573653.aspx). Možnost služby AD FS vyžaduje několik dalších kroků konfigurace.

Pokud se rozhodnete ukládat hodnoty hash v cloudu, budete mít a spustí nástroj pro synchronizaci adresářů, po kliknutí na **Další**.

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image18.png)

A za pár minut budete mít.

![Průvodce konfigurací nástroje synchronizace WAAD](single-sign-on/_static/image19.png)

Stačí ji spustit na jednom řadiči domény v organizaci na Windows serveru 2003 nebo vyšší. A není potřeba restartovat. Když to uděláte, všichni uživatelé jsou v cloudu a můžete provést jednotné přihlašování z jakéhokoli webových nebo mobilních aplikací pomocí SAML a OAuth, WS-Fed.

Někdy jsme získat požádáni o zabezpečené jde – Microsoft používá ho pro své vlastní citlivá firemní data? A odpověď zní: Ano, co děláme. Například, pokud přejdete na interní web Microsoft SharePoint na [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), výzva k přihlášení.

![Přihlášení k Office 365](single-sign-on/_static/image20.png)

Microsoft má povolenou služby AD FS, takže když zadáte ID Microsoftu, získat přesměruje na přihlašovací stránku služby AD FS.

![Přihlášení služby AD FS](single-sign-on/_static/image21.png)

A jakmile zadáte přihlašovací údaje uložené v interní účet Microsoft AD, máte přístup k této interní aplikace.

![MS Sharepointového webu](single-sign-on/_static/image22.png)

Používáme přihlášení serveru služby AD hlavně, protože již bylo předtím, než jsou dostupné služby Azure AD, ale proces přihlášení prochází adresář Azure AD v cloudu nastavenou službu AD FS. Jsme naše důležité dokumenty, správy zdrojového kódu, souborů správy výkonu, prodejní sestavy a informace, v cloudu a pomocí tohoto přesně stejné řešení je zabezpečit.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Vytvoření aplikace ASP.NET využívající Azure AD pro jednotné přihlašování

Visual Studio umožňuje skutečně snadno vytvořit aplikaci, která používá Azure AD pro jednotné přihlašování, jak je vidět z několika snímky obrazovky.

Když vytvoříte novou aplikaci ASP.NET, MVC nebo webového formuláře, je výchozí metodu ověřování ASP.NET Identity. Chcete-li změnit, která do služby Azure AD, kliknete **změnit ověřování** tlačítko.

![Změnit ověřování](single-sign-on/_static/image23.png)

Vyberte účty organizace, zadejte název domény a pak vyberte Single Sign On.

![Konfigurace dialog ověřování.](single-sign-on/_static/image24.png)

Můžete také poskytnout aplikaci pro čtení nebo pro čtení a zápis oprávnění pro adresář data. Pokud to uděláte, můžete použít [REST API služby Azure Graph](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) k vyhledání uživatelů telefonní číslo, zjistěte, pokud jsou v kanceláři, po poslední přihlášení na atd.

To je všechno, co musíte udělat – Visual Studio výzvu pro přihlašovací údaje pro správce pro vašeho tenanta Azure AD a pak nastaví váš projekt a tenanta Azure AD pro novou aplikaci.

Při spuštění projektu se zobrazí přihlašovací stránku a můžete se přihlásit pomocí přihlašovacích údajů uživatele v adresáři služby Azure AD.

![Přihlašovací účet organizace](single-sign-on/_static/image25.png)

![Přihlášen](single-sign-on/_static/image26.png)

Když nasadíte aplikaci do Azure, je vybrat vše, co musíte udělat **povolit ověřování organizace** zaškrtněte políčko, a ještě jednou všechny konfigurace za vás postará o Visual Studio.

![Publikování webu](single-sign-on/_static/image27.png)

Tyto snímky obrazovky pocházejí z úplný podrobný kurz, který ukazuje, jak vytvořit aplikaci, která používá ověřování Azure AD: [vývoj aplikací ASP.NET s použitím Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Souhrn

V této kapitole jste viděli, že Azure Active Directory, Visual Studio a technologie ASP.NET, umožňují snadno nastavit jednotné přihlašování v aplikacích Internet pro uživatelé ve vaší organizaci. Vaši uživatelé můžou přihlásit v aplikacích Internet pomocí stejných přihlašovacích údajů, které používají k přihlášení pomocí služby Active Directory v interní síti.

[Další kapitolu](data-storage-options.md) zkoumá možnosti úložiště dat, která je k dispozici pro cloudové aplikace.

<a id="resources"></a>
## <a name="resources"></a>Prostředky

Další informace naleznete v následujících materiálech:

- [Dokumentace ke službě Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Stránka portálu dokumentace ke službě Azure AD na webu windowsazure.com. Podrobné pokyny krok za krokem, najdete v článku **vývoj** oddílu.
- [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Stránka portálu dokumentaci ke službě Multi-Factor authentication v Azure.
- [Možnosti ověřování účtu organizace](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Vysvětlení možností ověřování Azure AD v dialogovém okně Nový projekt sady Visual Studio 2013.
- [Microsoft Patterns and Practices - federované Identity vzor](https://msdn.microsoft.com/library/dn589790.aspx).
- [Postupy: Instalace nástroje Azure Active Directory Sync](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 mapa obsahu](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Obsahuje odkazy na dokumentaci ke službě AD FS 2.0.
- [Ověřování založené na rolích a na základě seznamu ACL v aplikaci Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Ukázkové aplikace.
- [Blog o Azure Active Directory Graph API](https://blogs.msdn.com/b/aadgraphteam/).
- [Řízení přístupu v modelu BYOD a integrace adresáře v hybridní infrastrukturu identit](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Odborný Ed 2014 relace videa podle Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Předchozí](web-development-best-practices.md)
> [další](data-storage-options.md)
