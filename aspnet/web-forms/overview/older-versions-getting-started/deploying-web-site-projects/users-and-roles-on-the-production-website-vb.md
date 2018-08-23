---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: Uživatelé a role na provozním webu (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Nástroj pro správu webu technologie ASP.NET (WSAT) poskytuje webové uživatelské rozhraní pro konfiguraci nastavení členství a role a k vytváření, úpravy,...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: f63d64532543da681fdf88399d7dd365804674c4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752835"
---
<a name="users-and-roles-on-the-production-website-vb"></a>Uživatelé a role na provozním webu (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> Nástroj pro správu webu technologie ASP.NET (WSAT) poskytuje webové uživatelské rozhraní pro konfiguraci nastavení členství a rolí a pro vytváření, úpravy a odstraňování uživatelů a rolí. Bohužel WSAT funguje pouze v případě uživatel přejde v localhost, což znamená, že nelze dosáhnout nástroj pro správu webu produkční prostřednictvím prohlížeče. Dobrou zprávou je, že jsou alternativní postupy, které umožňují spravovat uživatele a role v produkčním prostředí. V tomto kurzu zjistí těchto alternativních řešení a dalších.


## <a name="introduction"></a>Úvod

ASP.NET 2.0 zavádí řadu *aplikačních služeb*, které představují sadu stavebních bloků služby, které můžete přidat do vaší webové aplikace. Přidali jsme členství a rolí služby na webu knihy kontroly zpátky [ *konfigurace na webu, že používá aplikační služby* kurzu](configuring-a-website-that-uses-application-services-vb.md). Členství služby usnadňuje vytváření a správa uživatelských účtů; Tato služba role nabízí rozhraní API pro zařazení do skupin uživatelů. Lokalita recenzí má tři uživatelské účty – Scott, Jisun a Alice – a s jedinou rolí správce se Scottem a Jisun v roli správce.

ASP. SÍŤ pro aplikační služby nejsou vázané na konkrétní implementaci. Místo toho dáte pokyn aplikační služby, které chcete použít konkrétní *poskytovatele*, a tohoto zprostředkovatele implementuje pomocí konkrétní technologii. Jsme nakonfigurovali recenzí webové aplikace pro použití `SqlMembershipProvider` a `SqlRoleProvider` zprostředkovatelů pro členství a rolí služby. Tyto dva poskytovatelé ukládat informace o účtu a role uživatele v databázi systému SQL Server a jsou nejčastěji používané zprostředkovatelé pro internetové webové aplikace hostované na webhosting společnosti.

Běžné výzvu pro vývojáře, kteří používají služby členství a rolí je Správa uživatelů a rolí v produkčním prostředí. Jak je odstranění uživatelského účtu z webu produkčního prostředí, přidejte novou roli nebo přidání stávajícího uživatele do stávající role? Tento kurz se věnuje různých postupů pro správu uživatelů a role na provozním webu.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Pomocí nástroje pro správu webu technologie ASP.NET

Technologie ASP.NET obsahuje [nástroje pro správu webu](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT), který umožňuje snadno vytvářet a spravovat uživatelské účty a role a k určování pravidel autorizace na základě uživatelů a rolí. Chcete-li použít WSAT, klikněte na ikonu konfigurace ASP.NET v Průzkumníku řešení nebo přejděte do nabídky webu nebo projekt a zvolte možnost konfigurace technologie ASP.NET. Kterýkoliv přístup spustí webový prohlížeč a odkazuje na WSAT na adrese jako: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT je rozdělený do tří částí:

- **Zabezpečení** – spravovat uživatele, role a pravidla autorizace.
- **ApplicationConfiguration** – Správa &lt;appSettings&gt; a nastavení protokolu SMTP z tohoto umístění. Můžete lze také převést aplikaci do režimu offline a spravovat ladění a trasování nastavení z tohoto, jakož i zadejte výchozí vlastní chybovou stránku.
- **ProviderConfiguration** – konfigurovat zprostředkovatele, který používá aplikační služby.

V části zabezpečení (ukazuje **obrázek 1**) obsahuje odkazy pro vytváření nových uživatelů, Správa uživatelů, vytváření a Správa rolí nebo vytváření a správa pravidla přístupu. Odsud můžete přidat novou roli systému, odstranit stávajícího uživatele, nebo přidat či odebrat role z určitého uživatelského účtu.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Obrázek 1**: části WSAT zabezpečení obsahuje možnosti pro správu uživatelů a rolí  
([Kliknutím ji zobrazíte obrázek v plné velikosti](users-and-roles-on-the-production-website-vb/_static/image3.png))

Bohužel WSAT je dostupná jenom místně. WSAT nelze najdete na webu produkčnímu; Pokud navštívíte `www.yoursite.com/asp.netwebadminfiles/default.aspx` získáte odpovědi 404 nebyl nalezen. Kód, který využívá používá WSAT `Membership` a `Roles` třídy v rozhraní .NET Framework pro vytváření, upravování a odstraňování uživatelů a rolí. Tyto třídy najdete informace o konfiguraci webové aplikace určit zprostředkovatele se použije. zpátky [ *konfigurace na webu, že používá aplikační služby* kurzu](configuring-a-website-that-uses-application-services-vb.md) nastavíme na webu knihy revize používat `SqlMembershipProvider` a `SqlRoleProvider` poskytovatelů. To zavedením přidání `<membership>` a `<roleManager>` oddíly na `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Všimněte si, že `<membership>` a `<roleManager>` sections – odkaz `SqlMembershipProvider` a `SqlRoleProvider` poskytovatelů v jejich `type` atribut v uvedeném pořadí. Tito poskytovatelé úložiště uživatelů a informací o rolích v zadané databázi systému SQL Server. Databázi použitou vydavatelem těchto poskytovatelů je určený `connectionStringName` atribut `ReviewsConnectionString`, který je definován v `~/ConfigSections/databaseConnectionStrings.config` souboru. Vzpomeňte si, že `databaseConnectionStrings.config` soubor ve vývojovém prostředí obsahuje připojovací řetězec k databázi vývoj, že `databaseConnectionStrings.config` souboru v produkčním prostředí obsahuje připojovací řetězec k produkční databázi.

Řečeno v kostce, WSAT musí být místně prostřednictvím vývojové prostředí, a funguje s informací o uživatelích a role v zadané v databázi `databaseConnectionStrings.config` souboru. V důsledku toho Pokud změníme informace o připojovacím řetězci v `databaseConnectionStrings.config` soubor ve vývojovém prostředí můžeme použít WSAT místně pro správu uživatelů a rolí v produkčním prostředí.

K ilustraci této funkce, otevřete `databaseConnectionStrings.config` souboru v sadě Visual Studio ve vývojovém prostředí a nahraďte řetězec připojení k databázi vývoj řetězec připojení k produkční databázi. Pak spusťte WSAT, přejděte na kartu zabezpečení a přidání nového uživatele s názvem Sam se heslo "password"! (menší uvozovky). **Obrázek 2** při vytváření tohoto účtu se zobrazuje obrazovka WSAT.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Obrázek 2**: vytvoření nového uživatele s názvem Sam v provozním prostředí  
([Kliknutím ji zobrazíte obrázek v plné velikosti](users-and-roles-on-the-production-website-vb/_static/image6.png))

Protože jsme změnili připojovací řetězec v `databaseConnectionStrings.config` tak, aby odkazoval na produkční server databáze Sam se přidal jako uživatele v produkčním prostředí. Chcete-li to ověřit, změňte připojovací řetězec v `databaseConnectionStrings.config` souborů zpět do vývoje databáze a potom navštivte `Login.aspx` stránky ve vývojovém prostředí. Zkuste se přihlásit jako Sam (viz **obrázek 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Obrázek 3**: nemůžete se přihlásit jako Sam ve vývojovém prostředí  
([Kliknutím ji zobrazíte obrázek v plné velikosti](users-and-roles-on-the-production-website-vb/_static/image9.png))

Nemůžete se přihlásit jako Sam ve vývojovém prostředí informace o uživatelském účtu v místní databázi neexistuje. Místo toho se přidal do provozní databáze. Chcete-li to ověřit, zobrazit obsah `aspnet_Users` tabulku v databázích vývoj i produkci. Ve vývojovém prostředí by měl být pouze tři záznamy pro uživatele Scott, Jisun a Alice. Ale `aspnet_Users` tabulce do provozní databáze má čtyři záznamy: Scott, Jisun, Alice a Sam. V důsledku toho Sam přihlásit na webových stránkách v produkčním prostředí, ale ne prostřednictvím vývojové prostředí.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Obrázek 4**: Sam se můžete přihlásit na provozním webu  
([Kliknutím ji zobrazíte obrázek v plné velikosti](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> Nezapomeňte změnit připojovací řetězec `databaseConnectionStrings.config` souboru zpět do databáze vývoje je připojovací řetězec, až to budete mít práce s jinak WSAT budete pracovat s použitím provozních dat při testování webu prostřednictvím vývoje prostředí. Také vzít v úvahu, zatímco technika, kterou jsme právě probírali nám umožňuje používat WSAT ke vzdálené správě uživatelů a rolí, měnit změny na žádné z ostatních WSAT konfigurace možností (pravidla přístupu, SMTP nastavení, ladění a trasování nastavení a tak dále) `Web.config` souboru. V důsledku toho všechny změny provedené nastavení použijí do vývojového prostředí a ne do produkčního prostředí.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Vytvoření vlastního uživatele a roli správy webové stránky

WSAT poskytuje výstupního pole systému pro správu uživatelů a rolí, ale jde spustit jenom místně a vyžaduje změny informací o připojovacím řetězci za účelem správy uživatelů a rolí v produkčním prostředí. Většina webů, které podporují uživatelské účty obsahovat také počet uživatelů a rolí správy webové stránky, které umožňují správcům ke správě uživatelů a rolí ze stránek v rámci stránky. Tyto stránky správy na základě webové bylo mnohem snazší správu uživatelů a rolí a jsou nezbytné pro weby tam, kde může být mnoho správci nebo správci, kteří nemají přístup k nebo technické znalosti potřebné k použití sady Visual Studio ke spuštění WSAT.

Technologie ASP.NET obsahuje řadu integrovaných ovládacích prvků související s přihlášením Web využívající prováděcí mnohé z těchto stejně snadné jako přetažení pro správu webových stránek. Můžete například vytvořit stránku pro správce vytvořit nový uživatelský účet přetažení ovládacím prvku CreateUserWizard na stránku a nastavit několik vlastností. Ve skutečnosti, na stránce pro vytváření uživatelů v WSAT ukazuje **obrázek 2** používá stejný CreateUserWizard ovládací prvek, který můžete přidat na stránky. Kromě toho jsou k dispozici prostřednictvím kódu programu prostřednictvím členství a rolí služby funkce `Membership` a `Roles` třídami v rozhraní .NET Framework. Pomocí těchto tříd můžete napsat kód, vytvářet, upravovat a odstraňovat uživatelů a rolí, a jde o přidání nebo odebrání uživatelů k rolím, chcete-li zjistit, co jsou uživatelé v rolích a provádět další úlohy související s uživateli a rolemi.

V [ *konfigurace na webu, že používá aplikační služby* kurzu](configuring-a-website-that-uses-application-services-vb.md) po přidání stránku `Admin` složku s názvem `CreateAccount.aspx`. Tato stránka umožňuje správci přidat nový uživatelský účet k lokalitě a k určení, zda bude nově vytvořený uživatel je v roli správce (viz **obrázek 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Obrázek 5**: Správci mohou vytvářet nové uživatelské účty  
([Kliknutím ji zobrazíte obrázek v plné velikosti](users-and-roles-on-the-production-website-vb/_static/image15.png))

Podrobnější rozbor vytváření uživatele a roli stránky pro správu, spolu s podrobné pokyny k používání `Membership` a `Roles` třídy a ovládacích prvků technologie ASP.NET související s přihlášením, nezapomeňte si přečíst Moje [zabezpečení webu Kurzy](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Zde najdete pokyny, jak sestavit webové stránky pro vytvoření nové účty, vytvoření a Správa rolí, přiřazování uživatelů k rolím a další běžné úlohy správy.

K implementaci funkcionality jako WSAT na provozním webu můžete vždy sestavovat vlastní řadu webových stránek, které implementují funkce WSAT. Abychom mohli začít, podívejte se na zdrojový kód, který je umístěn ve složce WSAT `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Další možností je použít alternativní WSAT Clem daň, která sdílí v tomto článku, [se zajištěním provozu svůj vlastní nástroj pro správu webu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). DaN vás čtenáře proces vytváření vlastního nástroje WSAT podobném, obsahuje zdrojový kód jeho aplikace ke stažení (v jazyce C#) a poskytuje podrobné pokyny pro přidání své vlastní WSAT na hostovaný web.

## <a name="summary"></a>Souhrn

Nástroj pro správu webu technologie ASP.NET (WSAT) lze použít v kombinaci s aplikačními službami členství a rolí ke správě informací o uživatelích a role pro váš web. Bohužel WSAT je přístupný jenom místně a nemůže být zobrazeny z produkčního webu. Však změnou připojovacího řetězce při vývoji prostředí tak, aby odkazoval na provozní databáze použít WSAT pro správu uživatelů a role na provozním webu.

Zatímco WSAT přístup nabízí rychlý a snadný způsob, jak spravovat uživatele a role, vyžaduje spuštění WSAT ze sady Visual Studio, stejně jako dočasné změny na informace o připojovacím řetězci. WSAT nabízí rychlý způsob, jak spravovat uživatele a role v produkčním prostředí, ale je náročné a nebude fungovat dobře pro weby s několika správci nebo správci, kteří nemají nebo nejsou obeznámeni s Visual Studio a WSAT. Většina webů, které podporují uživatelské účty z těchto důvodů zahrnují sadu pro správu webových stránek. Sada webových stránek se eliminuje potřeba WSAT a použít různé administrativní uživatelé z libovolného počítače.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Zkoumání ASP. Členství, role a profilu sítě.](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Vlastní nástroje pro správu webu se zajištěním provozu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Přehled nástroje pro správu webu](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Kurzy zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Předchozí](precompiling-your-website-vb.md)
