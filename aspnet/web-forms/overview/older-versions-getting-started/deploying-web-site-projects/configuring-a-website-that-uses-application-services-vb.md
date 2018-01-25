---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: "Konfigurace webu, který používá aplikačních služeb (VB) | Microsoft Docs"
author: rick-anderson
description: "Verze technologie ASP.NET 2.0 zavedl řadu aplikační služby, které jsou součástí rozhraní .NET Framework a slouží jako sada stavebním blokem služby, které je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 5f908eb6c6b2d18c6c41870a38bb618737949b0a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>Konfigurace webu, který používá aplikačních služeb (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> Verze technologie ASP.NET 2.0 zavedl řadu aplikační služby, které jsou součástí rozhraní .NET Framework a sloužit jako sada služeb stavebních bloků, které můžete použít k přidání bohaté funkce do webové aplikace. V tomto kurzu jsou zde popsány konfiguraci webu v provozním prostředí použít službu pro aplikace a řeší běžné problémy s Správa uživatelských účtů a rolí v provozním prostředí.


## <a name="introduction"></a>Úvod

Verze technologie ASP.NET 2.0 zavedl řadu *aplikačních služeb*, které jsou součástí rozhraní .NET Framework a slouží jako sada stavebním blokem služeb, které můžete použít k přidání bohaté funkce k vaší webové aplikaci. Aplikace služby patří:

- **Členství** – rozhraní API pro vytváření a správě uživatelských účtů.
- **Role** – rozhraní API pro zařazení do skupin uživatelů.
- **Profil** – rozhraní API pro ukládání obsahu vlastní, specifický pro uživatele.
- **Lokality mapy** – rozhraní API pro definování logické struktury lokality s ve formě hierarchii, což pak lze zobrazit pomocí ovládací prvky pro navigaci, jako jsou nabídky a cesta ke stránce.
- **Přizpůsobení** – rozhraní API pro údržbu vlastní předvolby, nejčastěji používá u [ *webové části*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitorování stavu** – rozhraní API pro sledování výkonu, zabezpečení, chyb a další metriky stavu systému pro spuštěné webovou aplikaci.
  

Rozhraní API aplikační služby nejsou svázané s konkrétní implementace. Místo toho dáte pokyn aplikačních služeb používat konkrétní *zprostředkovatele*, a tohoto zprostředkovatele implementuje pomocí konkrétní technologie. Nejčastěji používané zprostředkovatele pro internetové webové aplikace hostované na webových hostitelských společnosti jsou těchto zprostředkovatelů, kteří používají na implementace databáze systému SQL Server. Například `SqlMembershipProvider` je zprostředkovatel pro rozhraní API členství, která ukládá informace o uživatelském účtu v databázi Microsoft SQL Server.

Pomocí aplikace služeb a poskytovatele serveru SQL Server přidá některé běžné problémy při nasazení aplikace. Pro začátek musí být databázové objekty služby aplikace správně vytvořen pro vývoj a provozní databáze a správně inicializovat. Existují také důležité konfigurační nastavení, které je potřeba provést.

> [!NOTE]
> Rozhraní API byly navrženy používání aplikační služby [ *zprostředkovatele modelu*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), návrhový vzor, který umožňuje rozhraní API s podrobnosti implementace za běhu, musíte zadat. Rozhraní .NET Framework se dodává s počtem poskytovatelů služeb aplikací, které lze použít, jako `SqlMembershipProvider` a `SqlRoleProvider`, které jsou poskytovatelé pro členství a rolí rozhraní API, které používají SQL Server databáze implementace. Můžete také vytvořit a modul plugin vlastního zprostředkovatele. Ve skutečnosti kniha recenze webové aplikace už obsahuje vlastního zprostředkovatele pro rozhraní API Map lokality (`ReviewSiteMapProvider`), který vytvoří mapy webu z dat v `Genres` a `Books` tabulky v databázi.


V tomto kurzu začíná podívejte se na tom, jak I rozšířené je kniha recenze webové aplikace pomocí rozhraní API rolí a členství. Potom se provede nasazení webové aplikace, které používá aplikace služby s implementací databáze serveru SQL Server a řešení obvyklých potíží s Správa uživatelských účtů a rolí v produkčním prostředí se ukončí.

## <a name="updates-to-the-book-reviews-application"></a>Aktualizace aplikace recenze adresář

V posledních několika dokončit kurzů, které webové aplikace zkontroluje adresáře byl aktualizován ze statického webu pro dynamické a datové webové aplikace se sadou správy stránky pro správu žánry a recenze. Ale momentálně není chráněn této části Správa – každý uživatel, který zná (nebo odhadne) adresu URL stránky správy můžete waltz v a vytvořit, upravit nebo odstranit recenze na svém webovém serveru. Běžný způsob, jak chránit některé části webu je implementace uživatelské účty a pak pomocí autorizačních pravidel adres URL můžete omezit přístup na určité uživatele nebo role. Webová aplikace recenze adresáře, které jsou k dispozici ke stažení v tomto kurzu podporuje uživatelských účtů a rolí. Má jedné role definované s názvem správce a na stránkách správy přístup pouze uživatelé v této roli.

> [!NOTE]
> I jste vytvořili tři uživatelské účty ve webové aplikaci kniha recenze: Scott, Jisun a Alice. Všechny tři uživatelé mají stejné heslo: **heslo!** Scott Jisun je v roli správce a není Alice. Stránky webu s bez správy jsou stále dostupné anonymním uživatelům. To znamená, není nutné se přihlásit k přejděte na web, pokud chcete spravovat, v takovém případě musíte se přihlásit jako uživatel v roli správce.


Stránky předlohy recenze adresáře aplikace s je aktualizovaná tak, aby patří jiné uživatelské rozhraní pro ověřený a anonymní uživatele. Pokud anonymní uživatel navštíví web zobrazí přihlašovací odkaz v pravém horním rohu. Ověřený uživatel, zobrazí se zpráva, "Vítejte zpátky, *uživatelské jméno*!" a odkaz na odhlášení. Zde s také přihlašovací stránku (`~/Login.aspx`), obsahující webové přihlášení ovládací prvek, který poskytuje uživatelské rozhraní a logiku pro ověřování návštěvníka. Pouze správci mohou vytvářet nové účty. (Existuje stránek pro vytváření a správě uživatelských účtů v `~/Admin` složky.)

### <a name="configuring-the-membership-and-roles-apis"></a>Konfigurace členství a rolí rozhraní API

Webová aplikace kniha recenze používá členství a rolí rozhraní API pro podporu uživatelské účty a skupiny těchto uživatelů do rolí (konkrétně, roli správce). `SqlMembershipProvider` a `SqlRoleProvider` třídy zprostředkovatele se používají, protože potřebujete ukládat informace o účtu a role v databázi systému SQL Server.

> [!NOTE]
> V tomto kurzu není určen jako podrobné zkoumání v konfiguraci webové aplikace pro podporu členství a rolí rozhraní API. Pro důkladné podívejte se na tato rozhraní API a kroky, které je třeba provést nakonfigurovat web, který chcete používat, přečtěte si prosím Moje [ *kurzy zabezpečení webu*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


K použití služby pro aplikace s databází systému SQL Server je nejprve nutno přidat databázové objekty, které používají tyto poskytovatelé do databáze, kam chcete uživatelský účet a uložené informace o rolích. Tyto objekty požadavků databáze patří různých tabulek, zobrazení a uložených procedur. Pokud není uvedeno jinak, `SqlMembershipProvider` a `SqlRoleProvider` třídy zprostředkovatele používat SQL Server Express Edition databáze s názvem `ASPNETDB` umístěný v aplikaci s `App_Data` složky; pokud takové databáze neexistuje, je automaticky vytvořen s objekty nezbytné databáze tyto zprostředkovatelé za běhu.

Je možné, a obvykle ideální, chcete-li vytvořit aplikaci služby databázové objekty ve stejné databázi se uloží data webové stránky s konkrétní aplikace. Rozhraní .NET Framework se dodává s verzí nástroje s názvem `aspnet_regsql.exe` instalující databázové objekty zadanou databází. Mám zmizel dopředu a použít tento nástroj pro přidání tyto objekty `Reviews.mdf` databáze `App_Data` složky (vývoj databáze). Uvidíme použití tohoto nástroje později v tomto kurzu při přidáme tyto objekty k provozní databázi.

Pokud přidáte aplikace služby databázové objekty do databáze jiné než `ASPNETDB` budete muset přizpůsobit `SqlMembershipProvider` a `SqlRoleProvider` zprostředkovatele třídy konfigurace tak, aby využívaly příslušné databáze. Chcete-li přizpůsobit přidat zprostředkovatele členství [  *&lt;členství&gt; element* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) v rámci `<system.web>` v tématu `Web.config`; použít [  *&lt;roleManager&gt; element* ](https://msdn.microsoft.com/library/ms164660.aspx) konfigurace zprostředkovatele rolí. Následující fragment kódu jsou převzaty z s aplikací kniha recenze `Web.config` a zobrazuje nastavení konfigurace pro členství a rolí rozhraní API. Všimněte si, že oba registraci nového poskytovatele - `ReviewMembership` a `ReviewRole` -využívající `SqlMembershipProvider` a `SqlRoleProvider` poskytovatelů, v uvedeném pořadí.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

`Web.config` Soubor s `<authentication>` element také byl nakonfigurován pro podporu ověřování pomocí formulářů.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Omezení přístupu k stránek pro správu

Technologie ASP.NET umožňuje snadno udělit nebo odepřít přístup k určitého souboru nebo složky podle uživatele nebo podle role pomocí jeho *autorizace adres URL* funkce. (Jsme probrali stručně autorizace adres URL v *základní rozdíly mezi IIS a ASP.NET Development Server* kurzu a vám ukázal, jak používat službu IIS a ASP.NET Development Server autorizačních pravidel adres URL pro statické porovnání dynamický obsah.) Vzhledem k tomu, že chcete zakázat přístup k `~/Admin` složku s výjimkou těchto uživatelů v roli správce, je potřeba přidat autorizačních pravidel adres URL do této složky. Konkrétně autorizačních pravidel adres URL muset povolit uživatelům v roli správce a zakázat všichni ostatní uživatelé. To je možné udělat přidáním `Web.config` do souboru `~/Admin` složka s tímto obsahem:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Pro další informace o funkci autorizace adresu URL s ASP.NET a způsobu jeho použití napsat se autorizační pravidla pro uživatele a role, nezapomeňte si přečíst [ *autorizace na základě uživatele* ](../../older-versions-security/membership/user-based-authorization-vb.md) a [ *Autorizace na základě rolí* ](../../older-versions-security/roles/role-based-authorization-vb.md) kurzy z mé [ *kurzy zabezpečení webu*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Nasazení webové aplikace, která používá aplikačních služeb

Při nasazování webu, který používá aplikace služby a zprostředkovatele, který ukládá informace o aplikaci služby do databáze, je nutné vytvořený databázové objekty, které vyžaduje aplikační služby na produkční databázi. Původně provozní databáze neobsahuje tyto objekty, tak při prvním nasazení aplikace (nebo po nasazení poprvé po přidání aplikačních služeb), je nutné provést další kroky získat tyto objekty požadavků databáze na produkční databázi.

Dalším problémem mohou nastat při nasazování webu, který používá aplikační služby, pokud chcete replikovat uživatelské účty vytvořené ve vývojovém prostředí do produkčního prostředí. V závislosti na konfiguraci členství a role je možné, že i v případě, že zkopírujete úspěšně uživatelské účty, které byly vytvořeny ve vývojovém prostředí do provozní databáze, tito uživatelé nemůžete se přihlásit do webové aplikace v produkčním prostředí. Jsme budete podívejte se na příčinou tohoto problému a popisují postup situaci zabránit, aby ji.

ASP.NET se dodává s dobrý [ *webu správy nástroj (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) , může být spuštěn ze sady Visual Studio a umožňuje uživateli účtu, rolí a autorizační pravidla ke správě přes webové rozhraní. Bohužel WSAT funguje pouze pro místní weby, znamená, že nelze použít ke vzdálené správě uživatelských účtů, rolí a autorizační pravidla pro webovou aplikaci v produkčním prostředí. Podíváme různé způsoby, jak implementovat WSAT jako chování z provozního webu.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Přidání aspnet databázi objekty pomocí\_regsql.exe

*Nasazení databáze* kurz vám ukázal, jak zkopírovat tabulky a data z databáze vývoj do provozní databáze, a tyto postupy lze určitě zkopírovat objekty databáze aplikace služby, které se produkční databázi. Další možností je `aspnet_regsql.exe` nástroj, který přidá nebo odebere z databáze aplikace služby databázové objekty.

> [!NOTE]
> `aspnet_regsql.exe` Nástroj vytvoří databázové objekty zadanou databází. Nepřenese data v těchto objektů databáze z databáze vývoj provozní databázi. Pokud jste na mysli zkopírovat informace o uživatelském účtu a role v databázi vývoj do provozní databáze použít techniky popsané v *nasazení databáze* kurzu.


Umožní s, podívejte se na postup přidání databázové objekty do provozní databáze pomocí `aspnet_regsql.exe` nástroj. Začněte tím, že otevřete Průzkumníka Windows a přejdete k adresáři rozhraní .NET Framework verze 2.0 na počítači %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Existuje byste měli najít `aspnet_regsql.exe` nástroj. Tento nástroj můžete použít z příkazového řádku, ale zahrnuje také grafické uživatelské rozhraní; dvakrát klikněte `aspnet_regsql.exe` soubor spustíte jeho grafické součásti.

Nástroj spustí zobrazením úvodní obrazovka vysvětlením jeho účel. Na tlačítko Další zálohy na obrazovce "Vyberte možnost instalace", která je znázorněno na obrázku 1. Odsud můžete přidat aplikačních služeb databázové objekty, nebo je odebrat z databáze. Vzhledem k tomu, že nám chcete přidat tyto objekty do provozní databáze, vyberte možnost "Konfigurovat SQL Server pro služby aplikací" a klikněte na tlačítko Další.


[![Zvolte konfigurovat systém SQL Server pro služby aplikací](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Obrázek 1**: zvolit nakonfigurujte systém SQL Server pro služby aplikací ([Kliknutím zobrazit obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


"Vyberte a databáze serveru" obrazovky vyzve k zadání informace pro připojení k databázi. Zadejte databázový server, zabezpečovací přihlašovací údaje a název databáze, vám poskytne váš webového hostingu společnosti a klikněte na tlačítko Další.

> [!NOTE]
> Po zadání databázového serveru a pověření pravděpodobně dojde k chybě při rozšiřování databáze rozevíracího seznamu. `aspnet_regsql.exe` Nástroj dotazy `sysdatabases` systémové tabulky načíst seznam databází na serveru, ale některé webové hostování společností uzamčení jejich databázové servery tak, aby tyto informace nejsou veřejně dostupné. Pokud se tato chyba můžete zadat název databáze přímo do rozevíracího seznamu.


[![Zadejte nástroje s informací připojení databáze s](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Obrázek 2**: Zadejte nástroj s databáze s informace o připojení ([Kliknutím zobrazit obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


Na další obrazovce shrnuje akce, které se chystáte provést, a to, které objekty databáze služby aplikací se chystáte přidat do zadaná databáze. Klikněte na tlačítko Další dokončení této akce. Po chvíli se se poslední obrazovka zobrazí poznamenat, že databázové objekty byly přidány (viz obrázek 3).


[![Úspěšné! Aplikace služby databázové objekty, které byly přidány do provozní databáze](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Obrázek 3**: Úspěch! Aplikaci služby databáze objekty byly přidány do provozní databáze ([Kliknutím zobrazit obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


Pokud chcete ověřit, že aplikace služby databázové objekty byly úspěšně přidány do provozní databáze, otevřete SQL Server Management Studio a připojit k provozní databázi. Jak ukazuje obrázek 4, měli byste vidět tabulek databáze aplikace služby ve vaší databázi `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`, a tak dále.


[![Potvrďte, že databázové objekty byly přidány do provozní databáze](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Obrázek 4**: Ověřte, že databázové objekty byly přidány do provozní databáze ([Kliknutím zobrazit obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


Jenom je nutné použít `aspnet_regsql.exe` nástroj při nasazení webové aplikace poprvé nebo poprvé po spuštění pomocí aplikačních služeb. Jakmile jsou tyto objekty databáze na produkční databázi jejich won t potřeba znovu přidat ani upravit.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopírování uživatelské účty z vývojového do produkčního prostředí

Při použití `SqlMembershipProvider` a `SqlRoleProvider` třídy zprostředkovatele uložit informace o aplikaci služby v databázi serveru SQL, informace o uživatelském účtu a role je ukládány v různých tabulek databáze, včetně `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, a `aspnet_UsersInRoles`, mimo jiné. Pokud během vývoje vytvořit uživatelské účty ve vývojovém prostředí můžete replikovat tyto uživatelské účty v produkčním prostředí zkopírováním odpovídající záznamy z příslušných databázových tabulek. Pokud jste použili v Průvodci publikováním databáze aplikace služby databázové objekty, může také volbou pro kopírování záznamů, které by mělo za následek uživatelské účty vytvořené v vývoj také být v produkčním nasazení. Ale v závislosti na nastavení konfigurace, můžete zjistit, že tito uživatelé, jejíž účty byly vytvořeny při vývoji a zkopírovali do produkčního prostředí nelze se přihlásit z provozního webu. Co nabízí?

`SqlMembershipProvider` a `SqlRoleProvider` třídy zprostředkovatele byly navrženy tak, aby jedné databáze může sloužit jako úložiště uživatele pro více aplikací, kde každá aplikace může teoreticky mají uživatelé s překrývajícími se uživatelská jména a role se stejným názvem. Povolit pro tuto flexibilitu potřebují, databázi udržuje seznam aplikací `aspnet_Applications` tabulky a každý uživatel je přidružen jeden z těchto aplikací. Konkrétně `aspnet_Users` tabulka má `ApplicationId` sloupec, který přiřazuje každému uživateli záznam v `aspnet_Applications` tabulky.

Kromě `ApplicationId` sloupec, `aspnet_Applications` tabulka také obsahuje `ApplicationName` sloupec, který poskytuje lidské popisný název aplikace. Když pokusy webů pro práci s uživatelským účtem, jako je například ověřování pověření uživatele s z přihlašovací stránky, musíte upozornit `SqlMembershipProvider` třídy jaké aplikace pro práci s. Obvykle nemá to zadáním názvu aplikace a tato hodnota pochází z konfigurace zprostředkovatele s v `Web.config` – konkrétně prostřednictvím `applicationName` atribut.

Ale co se stane, když `applicationName` atribut není zadané v `Web.config`? V takovém případě členství systému používá kořenové cestě aplikace, jako `applicationName` hodnotu. Pokud `applicationName` atribut není explicitně nastavena v `Web.config`, pak je možné, že vývojové prostředí a provozním prostředí používají jinou aplikaci kořenové a proto bude spojený s jinou aplikaci názvy v aplikační služby. Pokud dojde k takové neshodě, pak bude mít tyto uživatelé vytvoření ve vývojovém prostředí `ApplicationId` hodnotu, která neodpovídá `ApplicationId` hodnotu pro produkční prostředí. Net výsledkem je, že tito uživatelé won t se moci přihlásit.

> [!NOTE]
> Pokud se přistihnete v této situaci - s uživatelskými účty, které jsou zkopírovány do ostrého provozu neodpovídající `ApplicationId` hodnota – můžete napsat dotaz aktualizovat tyto nesprávné `ApplicationId` hodnoty k `ApplicationId` použít na produkční. Po aktualizaci uživatelů, jejíž účty byly vytvořeny na vývojového prostředí nyní budou moct pro přihlášení do webové aplikace na produkční.


Dobrá zpráva je, že je jednoduchý krok může trvat dvě prostředí používat stejné `ApplicationId` – explicitně nastavit `applicationName` atribut `Web.config` u všech poskytovatelů služeb aplikací. Explicitně nastavit `applicationName` atribut "BookReviews" v `<membership>` a `<roleManager>` elementy jako tento fragment kódu ze `Web.config` zobrazuje.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Další informace o nastavení `applicationName` atribut a důvody, odkazovat na [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s příspěvku na blogu [ *vždy nastaven applicationName Vlastnost při konfiguraci členství technologie ASP.NET a dalších zprostředkovatelů*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Správa uživatelských účtů v produkčním prostředí

Nástroj pro správu webu ASP.NET (WSAT) umožňuje snadno vytvářet a spravovat uživatelské účty, definovat a použít role a pravopisu se autorizační pravidla založená na uživatele a role. Můžete spustit WSAT ze sady Visual Studio přejděte do Průzkumníka řešení a kliknutím na ikonu konfigurace ASP.NET nebo tak, že přejdete na webu nebo produktu Project nabídky a vyberete položku nabídky konfigurace technologie ASP.NET. Bohužel WSAT můžete pracovat pouze s místní weby. Proto nelze použít WSAT z pracovní stanice pro správu webu v provozním prostředí.

Dobrá zpráva je, že všechny funkce vystavené poskytované WSAT není k dispozici prostřednictvím kódu programu prostřednictvím členství a rolí rozhraní API; Kromě toho řadu obrazovky WSAT používat standardní ovládací prvky související s přihlášením ASP.NET. Stručně řečeno můžete přidat stránky ASP.NET na váš web, který nabízí možnosti potřeby správy.

Odvolat, aby starší kurzu aktualizovala kniha recenze webové aplikace zahrnout `~/Admin` složku a tato složka je nakonfigurovaný na Povolit jenom uživatelé v roli správce. Na stránce přidané do této složky s názvem `CreateAccount.aspx` z které správce může vytvořit nový uživatelský účet. Tuto stránku pomocí ovládacího prvku CreateUserWizard zobrazí logiku uživatelského rozhraní a back-end pro vytvoření nového uživatelského účtu. Jaké s více I přizpůsobit ovládací prvek zaškrtávací políčko, zda byl nový uživatel musí být také přidaní do role správce zahrnout (viz obrázek 5). S chvilku pracovní můžete vytvořit vlastní sadu stránek, který implementuje uživatele a role týkajících se správy úlohy, které by jinak poskytovaný WSAT.

> [!NOTE]
> Pro další informace o používání rozhraní API rolí a členství společně s ovládacích prvků technologie ASP.NET související s přihlášením, nezapomeňte si přečíst Moje [ *kurzy zabezpečení webu*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Další informace o přizpůsobení ovládacího prvku CreateUserWizard naleznete na [ *vytváření uživatelských účtů* ](../../older-versions-security/membership/creating-user-accounts-vb.md) a [ *ukládání Další informace o uživateli* ](../../older-versions-security/membership/storing-additional-user-information-vb.md) kurzy a podívejte se na [ *Erich Peterson* ](http://www.erichpeterson.com/) článek s [ *přizpůsobení ovládacího prvku CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Správci mohou vytvářet nové uživatelské účty](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Obrázek 5**: Správci mohou vytvořit nové uživatelské účty ([Kliknutím zobrazit obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


Pokud je třeba všechny funkce WSAT rezervaci [ *vrácení vaše vlastní nástroj Správa webu*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), ve které autor Dana Clem provede procesem vytvoření vlastní nástroj WSAT jako. Dana sdílí jeho aplikace s zdrojový kód (v jazyku C#) a poskytuje podrobné pokyny pro přidání do hostované webu.

## <a name="summary"></a>Souhrn

Při nasazení webové aplikace, která používá implementace databáze aplikace služby je třeba nejprve zajistit pro produkční databázi požadavků databázové objekty. Tyto objekty lze přidat pomocí technik popsaných v *nasazení databáze* kurzu; případně můžete použít `aspnet_regsql.exe` nástroje, jako jsme viděli v tomto kurzu. Jiné záležitosti, které jsme dotýkal v centru kolem synchronizaci použít v vývoj a produkční prostředí, (což je důležité, pokud chcete, aby uživatelé a role byly vytvořeny ve vývojovém prostředí platná u produkčních) a techniky pro název aplikace Správa uživatelů a rolí v provozním prostředí.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [*ASP.NET SQL Server Registration Tool (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Vytvoření databáze aplikace služby systému SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Vytvoření schématu členství v systému SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Zkoumání s členství technologie ASP.NET, role a profil*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Vrácení vlastní nástroj Správa webu*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Kurzy zabezpečení webu*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Přehled nástroje pro správu webu*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

>[!div class="step-by-step"]
[Předchozí](configuring-the-production-web-application-to-use-the-production-database-vb.md)
[další](strategies-for-database-development-and-deployment-vb.md)
