---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: Uživatelé a role na webu produkční (C#) | Microsoft Docs
author: rick-anderson
description: Nástroj pro správu webu ASP.NET (WSAT) poskytuje webové uživatelské rozhraní pro konfiguraci nastavení členství a role a k vytváření, úpravám...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: e3e1165959ae47715e0037db7a3bc6ac58807653
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888289"
---
<a name="users-and-roles-on-the-production-website-c"></a>Uživatelé a role na webu produkční (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> Nástroj pro správu webu ASP.NET (WSAT) poskytuje webové uživatelské rozhraní pro konfiguraci nastavení členství a role a pro vytváření, úpravy a odstraňování uživatelů a rolí. Bohužel WSAT pouze funguje, když navštívené z localhost, což znamená, že nebudete mít přístup nástroj pro správu webu produkční prostřednictvím prohlížeče. Dobrá zpráva je, že jsou alternativní postupy, které umožňují správu uživatelů a rolí na produkční. V tomto kurzu zjistí tato řešení a jiné.


## <a name="introduction"></a>Úvod

ASP.NET 2.0 zavedl řadu *aplikačních služeb*, sada služeb stavebních bloků, které můžete přidat do vaší webové aplikace, které jsou. Jsme přidali, členství a rolí služby Web knihy recenze zpátky [ *konfigurace na web, používá aplikace služby* kurzu](configuring-a-website-that-uses-application-services-cs.md). Službu členství usnadňuje vytváření a správa uživatelských účtů; služby rolí nabízí rozhraní API pro zařazení do skupin uživatelů. Web knihy recenze má tři uživatelské účty - Scott, Jisun a Alice - a s jedinou rolí správce, s Scott a Jisun v roli správce.

SOUBOR ASP. NET na aplikační služby nejsou svázané s konkrétní implementace. Místo toho dáte pokyn aplikačních služeb používat konkrétní *zprostředkovatele*, a tohoto zprostředkovatele implementuje pomocí konkrétní technologie. Jsme nakonfigurovali kniha recenze webovou aplikaci pro použití `SqlMembershipProvider` a `SqlRoleProvider` zprostředkovatele pro služby členství a rolí. Tyto dva poskytovatelé ukládat informace o účtu a role uživatele v databázi systému SQL Server a jsou nejčastěji používané zprostředkovatele pro internetové webové aplikace hostované na webových hostitelských společnosti.

Běžné výzvu pro vývojáře, kteří používají služby členství a role je Správa uživatelů a rolí v provozním prostředí. Jak budete odstranění uživatelského účtu z provozního webu, přidejte novou roli nebo přidání existujícího uživatele do existující role? V tomto kurzu zde popsány různé techniky pro správu uživatelů a rolí na produkční webu.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Pomocí nástroje Správa webu ASP.NET

Technologie ASP.NET obsahuje [nástroj Správa webu](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) umožňující snadno vytvořit a spravovat uživatelské účty a role a určit pravidla autorizace na základě uživatelů a rolí. Pokud chcete použít WSAT, klikněte na ikonu konfigurace ASP.NET v Průzkumníku řešení, nebo přejděte do nabídky webu nebo produktu Project a vyberte možnost konfigurace ASP.NET. Buď přístup spustí webový prohlížeč a odkazuje na WSAT na adresu jako: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

WSAT je rozdělené do tří částí:

- **Zabezpečení** -spravovat uživatele, role a autorizační pravidla.
- **ApplicationConfiguration** -spravovat &lt;appSettings&gt; a nastavení protokolu SMTP z tohoto umístění. Můžete můžete také do offline režimu aplikace a spravovat ladění a trasování nastavení z tohoto místa, a také určit výchozí vlastní chybovou stránku.
- **ProviderConfiguration** -konfigurovat zprostředkovatele používají služby aplikací.

V části zabezpečení (ukazuje **obrázek 1**) obsahuje odkazy pro vytváření nových uživatelů, správy uživatelů, vytvoření a Správa rolí a vytváření a správě přístupová pravidla. Odsud můžete přidat novou roli systému, odstranit stávajícího uživatele, nebo přidat nebo odebrat role z určitého uživatelského účtu.

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

**Obrázek 1**: V části zabezpečení WSAT obsahuje možnosti pro správu uživatelů a rolí  
([Kliknutím zobrazit obrázek v plné velikosti](users-and-roles-on-the-production-website-cs/_static/image3.png))

Bohužel WSAT přístupný jenom místně. WSAT nelze najdete na webu vzdálené produkční; Pokud navštívíte `www.yoursite.com/asp.netwebadminfiles/default.aspx` získat odpovědi 404 nebyl nalezen. Kód, který zajišťuje používá WSAT `Membership` a `Roles` třídy v rozhraní .NET Framework, pokud chcete vytvořit, upravovat a odstraňovat uživatele a role. Tyto třídy najdete informace o konfiguraci webové aplikace do určete zprostředkovatele se použije. zpět v [ *konfigurace na web, používá aplikace služby* kurzu](configuring-a-website-that-uses-application-services-cs.md) nastavíme web knihy recenze použít `SqlMembershipProvider` a `SqlRoleProvider` zprostředkovatele. To bylo přidání `<membership>` a `<roleManager>` částech k `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

Všimněte si, že `<membership>` a `<roleManager>` částech odkaz `SqlMembershipProvider` a `SqlRoleProvider` zprostředkovatele v jejich `type` atribut v uvedeném pořadí. Tyto zprostředkovatele uložit uživatele a informace o rolích do zadaná databáze systému SQL Server. Databáze používané těchto poskytovatelů je zadána `connectionStringName` atribut `ReviewsConnectionString`, která je definována v `~/ConfigSections/databaseConnectionStrings.config` souboru. Odvolat, který `databaseConnectionStrings.config` soubor ve vývojovém prostředí obsahuje připojovací řetězec k databázi vývoj, zatímco `databaseConnectionStrings.config` soubor u produkčních obsahuje připojovací řetězec k provozní databázi.

Stručně řečeno, WSAT je nutno přistupovat místně prostřednictvím vývojového prostředí a funguje s informacemi o uživatele a role v zadané v databázi `databaseConnectionStrings.config` souboru. V důsledku toho pokud dojde ke změně informace o připojovacím řetězci v `databaseConnectionStrings.config` souboru na vývojového prostředí můžeme použít WSAT místně pro správu uživatelů a rolí v provozním prostředí.

Pro ilustraci tuto funkci, otevřete `databaseConnectionStrings.config` souborů v sadě Visual Studio na vývojového prostředí a nahraďte připojovací řetězec databáze vývoj produkční připojovací řetězec databáze. Spusťte WSAT, přejděte na kartu zabezpečení a přidat nového uživatele s názvem Sam heslem "heslo!" (menší uvozovky). **Obrázek 2** zobrazí na obrazovce WSAT při vytváření tohoto účtu.

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

**Obrázek 2**: Vytvořte nového uživatele s názvem Sam v produkčním prostředí  
([Kliknutím zobrazit obrázek v plné velikosti](users-and-roles-on-the-production-website-cs/_static/image6.png))

Protože jsme změnili připojovací řetězec v `databaseConnectionStrings.config` odkazoval na provozním serveru databáze, Sam byla přidána jako uživatel v provozním prostředí. Chcete-li to ověřit, změňte připojovací řetězec `databaseConnectionStrings.config` souboru zpět do databáze vývoj a potom navštivte `Login.aspx` stránky ve vývojovém prostředí. Zkuste se přihlásit jako Sam (viz **obrázek 3**).

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

**Obrázek 3**: nemůžete se přihlásit jako Sam ve vývojovém prostředí  
([Kliknutím zobrazit obrázek v plné velikosti](users-and-roles-on-the-production-website-cs/_static/image9.png))

Nemůžete se přihlásit jako Sam ve vývojovém prostředí vzhledem k tomu, že informace o uživatelském účtu v místní databázi neexistuje. Místo toho je byla přidána do provozní databáze. Chcete-li to ověřit, zobrazte obsah `aspnet_Users` tabulky v vývoj a provozní databáze. Ve vývojovém prostředí by měl být jenom tři záznamy pro uživatele Scott, Jisun a Alice. Ale `aspnet_Users` tabulky v databázi produkční má čtyři záznamy: Scott, Jisun, Alice a Sam. V důsledku toho Sam přihlásit prostřednictvím webu v produkčním prostředí, ale ne prostřednictvím vývojového prostředí.

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

**Obrázek 4**: Sam se můžete přihlásit na produkční webu  
([Kliknutím zobrazit obrázek v plné velikosti](users-and-roles-on-the-production-website-cs/_static/image12.png))

> [!NOTE]
> Nezapomeňte změnit připojovací řetězec `databaseConnectionStrings.config` soubor zpět do databáze vývoj je řetězec připojení po dokončení práce s jinak WSAT budete pracovat s použitím provozních dat při testování webu prostřednictvím vývoj prostředí. Mějte při technik, které jsme probrali umožňuje použít WSAT pro vzdálenou správu uživatelů a rolí, změny do žádné jiné WSAT možnosti konfigurace (pravidla přístupu, SMTP nastavení, ladění a trasování nastavení a tak dále) upravit `Web.config` souboru. V důsledku toho použít všechny změny nastavení vývojového prostředí a ne do produkčního prostředí.


## <a name="creating-custom-user-and-role-management-web-pages"></a>Vytvoření vlastního uživatele a Role správy webové stránky

WSAT poskytuje out pole systému pro správu uživatelů a rolí, ale může být spuštěn pouze místně a vyžaduje provedení změn informace o připojovacím řetězci za účelem správy uživatelů a rolí na produkční. Většina webů, které podporují uživatelské účty také obsahovat číslo uživatele a role správy webové stránky, které umožňují správcům spravovat uživatele a role ze stránky v rámci lokality. Tak správu na základě webové stránky bylo mnohem snazší správu uživatelů a rolí a je nezbytné pro lokality, kde může být mnoho správci nebo správci, kteří nemají přístup k nebo technické pozadí pomocí sady Visual Studio spustíte WSAT.

Technologie ASP.NET obsahuje řadu předdefinovaných související s přihlášením webové kontroly, které implementace řadu tyto webové stránky pro správu stejně snadná jako přetažení. Například můžete vytvořit na stránce pro správce vytvořit nový uživatelský účet pomocí přetahování CreateUserWizard ovládacího prvku na stránku a nastavení několik vlastností. V fakt, na stránce pro vytváření uživatelů v WSAT ukazuje **na obrázku 2** používá stejný CreateUserWizard ovládací prvek, který můžete přidat na stránky. Kromě toho jsou k dispozici prostřednictvím kódu programu prostřednictvím funkce členství a role služeb `Membership` a `Roles` třídy v rozhraní .NET Framework. S tyto třídy můžete napsat kód pro vytvářet, upravovat a odstraňovat uživatele a také role, přidávat nebo odebírat uživatele k rolím, chcete-li zjistit, co uživatelé jsou v jaké role a provést další úlohy související s uživatelem a role.

V [ *konfigurace na web, používá aplikace služby* kurzu](configuring-a-website-that-uses-application-services-cs.md) po přidání stránky `Admin` složku s názvem `CreateAccount.aspx`. Tato stránka umožňuje správci přidat nový uživatelský účet k lokalitě a k určení, zda je nově vytvořeného uživatele v roli správce (viz **obrázek 5**).

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

**Obrázek 5**: Správci mohou vytvářet nové uživatelské účty  
([Kliknutím zobrazit obrázek v plné velikosti](users-and-roles-on-the-production-website-cs/_static/image15.png))

Pro podrobnější podívejte se na vytváření uživatelů a rolí správy stránky, společně s podrobné pokyny týkající se použití `Membership` a `Roles` třídy a ovládacích prvků technologie ASP.NET související s přihlášením, nezapomeňte si přečíst Moje [zabezpečení webu Kurzy](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Existuje najdete pokyny o tom, jak sestavit webové stránky pro vytvoření nové účty, vytvoření a Správa rolí, přiřazování uživatelů do rolí a další běžné úlohy správy.

K implementaci WSAT funkcí podobných na produkční webu můžete vždy vytvořit vlastní řady webových stránek, které implementují funkce WSAT. Abyste mohli začít, podívejte se na WSAT zdrojový kód, který je umístěný ve složce `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Další možností je použít alternativní WSAT Dana Clem, který sdílí v jeho článku, [vrácení vaše vlastní nástroj Správa webu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dana provede čtečky provede procesem vytváření vlastní nástroj jako WSAT, obsahuje zdrojový kód jeho aplikace ke stažení (v jazyku C#) a poskytuje podrobné pokyny pro přidání jeho vlastní WSAT na hostované Web.

## <a name="summary"></a>Souhrn

Nástroj pro správu webu ASP.NET (WSAT) lze použít v kombinaci s aplikační služby členství a rolí ke správě uživatelů a rolí informace pro váš web. Bohužel se WSAT je k dispozici pouze místně a nemůže návštěvy z provozního webu. Však změnit připojovací řetězec pro vývoj prostředí tak, aby odkazoval na produkční databázi pomocí WSAT ke správě uživatelů a rolí na produkční webu.

Při přístupu WSAT poskytuje rychlý a snadný způsob, jak spravovat uživatele a role, vyžaduje spuštění WSAT ze sady Visual Studio, jakož i dočasný změny na informace o připojovacím řetězci. WSAT nabízí rychlý způsob, jak spravovat uživatele a role v produkčním prostředí, ale je náročná a není funguje dobře pro weby s několika správci nebo správci, kteří nemají nebo se s Visual Studio a WSAT. Většina webů, které podporují uživatelské účty z těchto důvodů zahrnují sadu pro správu webových stránek. Sada webových stránek eliminuje potřebu WSAT a použít různé administrativní uživatelé z libovolného počítače.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Zkoumání ASP. Členství, role a profil pro NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Vrácení vlastní nástroj Správa webu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Přehled nástroje pro správu webu](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Kurzy zabezpečení webu](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Předchozí](precompiling-your-website-cs.md)
> [další](asp-net-hosting-options-vb.md)
