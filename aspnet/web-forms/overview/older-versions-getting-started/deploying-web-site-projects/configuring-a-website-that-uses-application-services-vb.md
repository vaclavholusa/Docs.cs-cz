---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Konfigurace webu, který používá aplikační služby (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Verze technologie ASP.NET 2.0 zavedené řadě aplikačních služeb, které jsou součástí rozhraní .NET Framework a slouží jako sadu stavebních bloků služby, které yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 69c8bb4061c2ed7fcc44038aa3bf0268ac86c9ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387491"
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>Konfigurace webu, který používá aplikační služby (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> Verze technologie ASP.NET 2.0 zavedli řadu aplikační služby, které jsou součástí rozhraní .NET Framework a slouží jako sadu stavebních bloků služby, které vám umožní doplnit celou řadu funkcí do webové aplikace. Tento kurz popisuje, jak nakonfigurovat webovou stránku v provozním prostředí použít aplikačních služeb a řeší běžné problémy se správou uživatelské účty a role na provozním prostředí.


## <a name="introduction"></a>Úvod

Verze technologie ASP.NET 2.0 zavedené řadu *aplikačních služeb*, které jsou součástí rozhraní .NET Framework a slouží jako sadu stavebních bloků služby, které můžete doplnit celou řadu funkcí do webové aplikace. Aplikační služby zahrnují:

- **Členství** – rozhraní API pro vytváření a správa uživatelských účtů.
- **Role** – rozhraní API pro zařazení do skupin uživatelů.
- **Profil** – rozhraní API pro ukládání obsahu vlastní, specifické pro uživatele.
- **Mapa webu** – rozhraní API pro definování lokality s logickou strukturou v podobě hierarchie, které lze zobrazit pomocí navigační ovládací prvky, jako je například nabídek a s popisem cesty.
- **Přizpůsobení** – rozhraní API pro udržování vlastní předvolby, se nejčastěji používají [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitorování stavu** – rozhraní API pro monitorování výkonu, zabezpečení, chyby a jiné metriky stavu systému pro provozní webové aplikace.
  

Aplikační služby API nejsou vázané na konkrétní implementaci. Místo toho dáte pokyn aplikační služby, které chcete použít konkrétní *poskytovatele*, a tohoto zprostředkovatele implementuje pomocí konkrétní technologii. Nejčastěji používané zprostředkovatelé pro internetové webové aplikace hostované na webhosting společnosti jsou těchto zprostředkovatelů, které používají implementace databáze systému SQL Server. Například `SqlMembershipProvider` je zprostředkovatele pro členství v rozhraní API, které jsou uloženy informace o uživatelském účtu v databázi Microsoft SQL Server.

Použití aplikačních služeb a systému SQL Server zprostředkovatelů přidá některé běžné problémy při nasazování aplikace. Začínáte musí být objekty databáze služeb aplikací správně vytvořena pro vývoj i produkci databáze a správně inicializován. Existují také důležitá nastavení konfigurace, které je potřeba provést.

> [!NOTE]
> Rozhraní API byly navrženy pomocí aplikační služby [ *modelu poskytovatele*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), návrhový vzor, který umožňuje rozhraní API s podrobnosti implementace poskytované v době běhu. Rozhraní .NET Framework se dodává s celou řadou poskytovatelů služeb aplikací, které lze použít, jako `SqlMembershipProvider` a `SqlRoleProvider`, které jsou zprostředkovatelů pro členství a rolí API, která používá SQL Server databáze implementace. Můžete také vytvořit a modulu plug-in vlastního zprostředkovatele. Ve skutečnosti recenzí webové aplikace už obsahuje vlastního zprostředkovatele pro rozhraní API pro mapy webu (`ReviewSiteMapProvider`), což vytvoří mapa webu z dat v `Genres` a `Books` tabulky v databázi.


Tento kurz pracuje s podívat, jak můžu rozšířit recenzí webové aplikace pro použití rozhraní API rolí a členství. Potom se provede nasazení webové aplikace, který používá aplikační služby s implementací databáze systému SQL Server a řešení běžných problémů se správou uživatelské účty a role na provozním prostředí, uzavře.

## <a name="updates-to-the-book-reviews-application"></a>Aktualizace aplikace revize knihy

Za posledních několik kurzů, které webové aplikace zkontroluje knihy se aktualizoval z: statického webu k aplikaci dynamické weby s daty dokončení sadu pro správu žánry a revize stránky pro správu. Ale momentálně není chráněn této části Správa – každý uživatel, který zná (nebo uhádne) Adresa URL stránky pro správu můžete waltz v a vytvořit, upravit nebo odstranit recenze na našem webu. Běžný způsob, jak chránit některé části tohoto webu je k implementaci uživatelských účtů a pak pomocí autorizačních pravidel adres URL můžete omezit přístup na určité uživatele nebo role. Webová aplikace recenzí, které jsou k dispozici ke stažení v tomto kurzu podporuje uživatelských účtů a rolí. Má jedné definice role s názvem správce a jenom uživatelé s touto rolí mají přístup k stránky pro správu.

> [!NOTE]
> Můžu odebrat vytvoří tři uživatelské účty ve webové aplikaci recenzí: Scott, Jisun a Alice. Všechny tři uživatelé mají stejné heslo: **heslo!** Scott a Jisun jsou v roli správce, není Alice. Stránky webu s bez správy jsou stále dostupné pro anonymní uživatele. To znamená, nepotřebujete k přihlášení k webu, pokud chcete spravovat, v takovém případě musíte se přihlásit jako uživatel v roli správce.


Aktualizovali jsme stránky předlohy knihy revize aplikace s zahrnout jiného uživatelského rozhraní pro ověřený a anonymní uživatele. Pokud je anonymní uživatel navštíví web marcela vidí odkazu pro přihlášení v pravém horním rohu. Ověřený uživatel se zobrazí se zpráva "Vítejte zpět, *uživatelské jméno*!" a odkaz na odhlášení. Tam s také na přihlašovací stránce (`~/Login.aspx`), která obsahuje ovládací prvek webové přihlášení, která poskytuje uživatelské rozhraní a logiky za účelem ověřování totožnosti návštěvník. Pouze správci mohou vytvářet nové účty. (Nejsou stránky pro vytváření a správa uživatelských účtů v `~/Admin` složky.)

### <a name="configuring-the-membership-and-roles-apis"></a>Konfigurace členství a rolí rozhraní API

Webová aplikace recenzí používá členství a rolí rozhraní API pro podporu uživatelské účty a skupiny těchto uživatelů do rolí (konkrétně role správce). `SqlMembershipProvider` a `SqlRoleProvider` třídy zprostředkovatele se používají, protože chceme, aby k ukládání informací o účtu a rolích do databáze SQL serveru.

> [!NOTE]
> V tomto kurzu není určen jako podrobné zkoumání v konfiguraci webové aplikace pro podporu rozhraní API rolí a členství. Důkladný rozbor tato rozhraní API a kroky musíte provést konfiguraci webu k jejich použití, přečtěte si prosím Moje [ *kurzy o zabezpečení webu*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Použití aplikačních služeb s databází SQL Server, musíte nejprve přidat databázové objekty používané těchto poskytovatelů pro databáze, místo, kam chcete uživatelský účet a uložené informace o rolích. Tyto objekty požadované databáze zahrnují celou řadu tabulek, zobrazení a uložených procedur. Pokud není uvedeno jinak, `SqlMembershipProvider` a `SqlRoleProvider` třídy zprostředkovatele použití systému SQL Server Express Edition databázi s názvem `ASPNETDB` umístěný v aplikaci s `App_Data` složky; pokud takové databáze buď neexistuje, automaticky se vytvoří s nezbytné databázových objektů pomocí těchto zprostředkovatelů za běhu.

Je možné, a obvykle ideální, chcete-li vytvořit aplikaci služby databázové objekty ve stejné databázi na webu s specifické pro aplikace se mají ukládat data. Rozhraní .NET Framework se dodává s nástrojem s názvem `aspnet_regsql.exe` databázové objekty, které instaluje na zadanou databázi. Mám předem a použít tento nástroj k přidání tyto objekty `Reviews.mdf` databáze v `App_Data` složky (vývoj databáze). Uvidíme, jak používat tento nástroj později v tomto kurzu při přidáme tyto objekty do provozní databáze.

Pokud chcete přidat aplikace databázových objektů do databáze služby jiné než `ASPNETDB` budete muset přizpůsobit `SqlMembershipProvider` a `SqlRoleProvider` poskytovatele třídy konfigurace tak, že používají příslušné databáze. Přizpůsobení přidat zprostředkovatele členství [  *&lt;členství&gt; element* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) v rámci `<system.web>` tématu `Web.config`; použít [  *&lt;roleManager&gt; element* ](https://msdn.microsoft.com/library/ms164660.aspx) nakonfigurovat zprostředkovatele rolí. Následující fragment kódu je převzata z aplikací s recenzí `Web.config` a ukazuje, konfigurace nastavení pro rozhraní API rolí a členství. Všimněte si, že oba registraci nového poskytovatele - `ReviewMembership` a `ReviewRole` -, které používají `SqlMembershipProvider` a `SqlRoleProvider` poskytovatelů, v uvedeném pořadí.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

`Web.config` Soubor s `<authentication>` element je také nakonfigurovaný pro podporu ověřování pomocí formulářů.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Omezení přístupu k stránky pro správu

Technologie ASP.NET umožňuje snadno udělit nebo odepřít přístup k určitého souboru nebo složky podle uživatele nebo podle role pomocí jeho *autorizace adres URL* funkce. (Krátce jsme probírali autorizace adres URL v *základní rozdíly mezi IIS a serveru ASP.NET Development Server* kurzu a jsme si ukázali, jak použít službu IIS a serveru ASP.NET Development Server autorizačních pravidel adres URL pro statické oproti dynamického obsahu.) Protože chceme zakázat přístup k `~/Admin` složku s výjimkou tito uživatelé v roli správce, je potřeba přidat autorizačních pravidel adres URL do této složky. Konkrétně autorizačních pravidel adres URL muset povolit uživatelům v roli správce a odepřít jiným uživatelům. To lze provést přidáním `Web.config` do souboru `~/Admin` složka s následujícím obsahem:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Pro další informace o funkci ověřování adresy URL s ASP.NET a jak ji používat k pravopisu si autorizačních pravidel pro uživatele a role, nezapomeňte si přečíst [ *autorizace na základě uživatele* ](../../older-versions-security/membership/user-based-authorization-vb.md) a [ *Autorizace na základě rolí* ](../../older-versions-security/roles/role-based-authorization-vb.md) kurzů od Moje [ *kurzy o zabezpečení webu*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Nasazení webové aplikace, který používá aplikační služby

Při nasazování webu, který používá aplikační služby a zprostředkovatele, který ukládá informace o službách aplikací do databáze, je nutné vytvořené objekty databáze vyžaduje aplikační služby na produkční databázi. Zpočátku provozní databáze neobsahuje tyto objekty, tak při prvním nasazení aplikace (nebo po nasazení poprvé po přidání aplikační služby), musejí udělat dodatečné kroky k získání těchto nezbytných databázových objektů provozní databáze.

Dalším problémem může dojít při nasazování webu, který používá aplikační služby, pokud chcete provést replikaci uživatelské účty vytvořené ve vývojovém prostředí do produkčního prostředí. V závislosti na konfiguraci členství a rolí je možné, že i v případě, že zkopírujete úspěšně uživatelské účty, které byly vytvořeny ve vývojovém prostředí do provozní databáze, tito uživatelé nemůže přihlásit do webové aplikace v produkčním prostředí. Vytvoříme prohlédnout příčinu těchto potíží a diskuzi o tom, aby se zabránilo jejímu děje.

Technologie ASP.NET se dodává s nice [ *nástroj pro správu webového serveru (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) , který lze spustit ze sady Visual Studio a umožňuje uživatelům účtu, role a autorizační pravidla ke správě přes webové rozhraní. Bohužel WSAT funguje jenom pro místní weby, což znamená, že nelze použít ke vzdálené správě uživatelských účtů, rolí a pravidel autorizace pro webovou aplikaci v produkčním prostředí. Podíváme se na různé způsoby, jak implementovat podobně WSAT z produkčního webu.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Přidání aspnet databázi pomocí objektů\_regsql.exe

*Nasazení databáze* kurz vám ukázal, jak kopírovat tabulek a dat z databáze vývoje do provozní databáze a následujících postupů lze jistě objekty databáze aplikace služby k zkopírovat provozní databáze. Další možností je `aspnet_regsql.exe` nástroj, který přidá nebo odebere z databáze aplikace služby databázové objekty.

> [!NOTE]
> `aspnet_regsql.exe` Nástroj vytvoří databázové objekty v zadané databázi. Se nemigruje data v těchto objektů databáze z databáze vývoje do provozní databáze. Pokud jste v úmyslu kopírovat informace o uživatelském účtu a role databáze vývoje do provozní databáze použijte techniky popsané v *nasazení databáze* kurzu.


Umožní s, podívejte se na tom, jak přidat k produkční databázi pomocí databázových objektů `aspnet_regsql.exe` nástroj. Začněte tím, že otevřete Průzkumníka Windows a přejděte k adresáři rozhraní .NET Framework verze 2.0 ve vašem počítači %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Existuje byste měli najít `aspnet_regsql.exe` nástroj. Tento nástroj se dá použít z příkazového řádku, ale zahrnuje také grafické uživatelské rozhraní; dvakrát klikněte `aspnet_regsql.exe` souboru ke spuštění grafického komponenty.

Nástroj spustí zobrazením úvodní obrazovky s vysvětlením účelu. Klikněte na tlačítko vedle záloh na obrazovku "Vyberte možnost instalace", která je znázorněna na obrázku 1. Odsud můžete přidat databázové objekty aplikační služby nebo je odebrat z databáze. Vzhledem k tomu, že budeme chtít přidat tyto objekty k provozní databázi, vyberte možnost "Konfigurovat SQL Server pro aplikační služby" a klikněte na tlačítko Další.


[![Vyberte konfigurace systému SQL Server pro služby aplikací](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Obrázek 1**: Nakonfigurujte systém SQL Server se rozhodnete pro aplikační služby ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


V "Vyberte do serveru a databázi" obrazovky vyzve k zadání informace pro připojení k databázi. Zadejte databázový server, zabezpečovací přihlašovací údaje a název databáze, který vám poskytne váš webhosting společnosti a klikněte na tlačítko Další.

> [!NOTE]
> Po zadání vašeho databázového serveru a přihlašovací údaje mohou dojde k chybě při rozšiřování rozevírací seznam databází. `aspnet_regsql.exe` Nástroj pro dotazy `sysdatabases` systémová tabulka načíst seznam databází na serveru, ale některé webové hostování společnosti uzamknutí jejich databázové servery tak, aby tyto informace není veřejně k dispozici. Pokud se zobrazí tato chyba můžete zadat název databáze přímo do rozevíracího seznamu.


[![Zadat nástroj s informací o připojení databáze s](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Obrázek 2**: zadat nástroj s databáze s informací o připojení ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


Na další obrazovce shrnuje akce, které se chystáte provést, a to, které objekty databáze služeb aplikací se chystáte přidat k zadané databázi. Klikněte na tlačítko vedle dokončení této akce. Po chvíli se zobrazí na poslední obrazovce poznamenat, že databázové objekty byly přidány (servery viz obrázek 3).


[![Úspěch! Aplikace služby databázové objekty byly přidány do provozní databáze](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Obrázek 3**: Úspěch! Aplikaci služby databáze objekty byly přidány do provozní databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


Pokud chcete ověřit, že aplikace služby databázové objekty byly úspěšně přidány do provozní databáze, otevřete SQL Server Management Studio a připojte se k provozní databázi. Jak je vidět na obrázku 4, měli byste vidět tabulky databáze služeb aplikací ve vaší databázi `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`a tak dále.


[![Potvrďte, že databázové objekty byly přidány do provozní databáze](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Obrázek 4**: Ověřte, že databázové objekty byly přidány do provozní databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


Je potřeba jenom použít `aspnet_regsql.exe` nástroj při nasazení webové aplikace poprvé nebo poprvé po spuštění služby aplikace. Jakmile tyto databázové objekty na provozní databázi, vyhráli t potřeba znovu přidat ani upravit.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopírování uživatelské účty z vývojového do produkčního prostředí

Při použití `SqlMembershipProvider` a `SqlRoleProvider` třídy zprostředkovatele ukládat informace o službách aplikací do databáze serveru SQL, informace o uživatelském účtu a role je uložena v různých databázové tabulky, včetně `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, a `aspnet_UsersInRoles`, mimo jiné. Pokud během vývoje vytvoříte uživatelské účty ve vývojovém prostředí můžete replikovat tyto uživatelské účty v produkčním prostředí tak, že zkopírujete odpovídající záznamy z příslušných databázových tabulek. Pokud jste použili k nasazení aplikace služby databázové objekty Průvodce publikováním databáze může také rozhodnete kopírování záznamů, které by mělo za následek uživatelské účty vytvořené ve vývoji nebyla v produkčním prostředí. Ale v závislosti na nastavení konfigurace, může se stát, že tito uživatelé, jejichž účty byly vytvořeny ve vývoji a zkopírovali do produkčního prostředí nelze se přihlásit na webu produkčního prostředí. Co nabízí?

`SqlMembershipProvider` a `SqlRoleProvider` třídy zprostředkovatele byly navržené tak, aby izolované databáze může sloužit jako úložiště uživatele pro více aplikací, kde každá aplikace může teoreticky, mají uživatelé s překrývající se uživatelská jména a rolí se stejným názvem. Chcete-li povolit tuto flexibilitu, databázi udržuje seznam aplikací v `aspnet_Applications` tabulky a každý uživatel je přidružený nejméně k jednomu z těchto aplikací. Konkrétně `aspnet_Users` tabulka má `ApplicationId` sloupec, který přiřazuje každému uživateli záznam v `aspnet_Applications` tabulky.

Kromě `ApplicationId` sloupci `aspnet_Applications` tabulka obsahuje také `ApplicationName` sloupec, který obsahuje více lidských – popisný název pro aplikaci. Pokusy webu pro práci s uživatelským účtem, jako je například ověřování pověření uživatele s z přihlašovací stránky, musíte upozornit `SqlMembershipProvider` třídy, které aplikace pro spolupráci s. Obvykle dělá to zadáním názvu aplikace a je tato hodnota pochází z konfigurace poskytovatele s v `Web.config` – konkrétně prostřednictvím `applicationName` atribut.

Ale co se stane, když `applicationName` atribut není zadán v `Web.config`? V takovém případě členství systém používá kořenové cestě aplikace jako `applicationName` hodnotu. Pokud `applicationName` atribut není explicitně nastavena `Web.config`, pak je možné, že vývojovým prostředím a produkčním prostředí použít jiné aplikační kořen a proto mají být asociována s novou aplikaci názvy v aplikační služby. Případě Tato neshoda pak tito uživatelé vytvoření ve vývojovém prostředí budou mít `ApplicationId` hodnotu, která se neshoduje s `ApplicationId` hodnotu pro produkční prostředí. Net výsledkem je, že tito uživatelé vyhráli t se moct přihlásit.

> [!NOTE]
> Pokud se v této situaci – s uživatelskými účty, které jsou zkopírovány do ostrého provozu neshodný `ApplicationId` hodnota – můžete napsat dotaz k aktualizaci těchto nesprávné `ApplicationId` hodnoty `ApplicationId` používat v produkčním prostředí. Po aktualizaci uživatele, jejichž účty byly vytvořeny ve vývojovém prostředí teď bude moct přihlásit do webové aplikace v produkčním prostředí.


Dobrou zprávou je, že je jednoduchý krok můžete provést k zajištění, že dvě prostředí použít stejné `ApplicationId` – je explicitně nastaveno `applicationName` atribut `Web.config` pro všech zprostředkovatelů služby vaší aplikace. Explicitně nastavit `applicationName` atribut na "BookReviews" `<membership>` a `<roleManager>` prvky jako tento fragment kódu z `Web.config` ukazuje.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Další informace o nastavení `applicationName` atribut a úvah, odkazovat na [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) blogovém příspěvku s [ *vždy nastavena vlastnost applicationName Vlastnost při konfiguraci členství technologie ASP.NET a dalších poskytovatelů*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Správa uživatelských účtů v provozním prostředí

Nástroj pro správu webu technologie ASP.NET (WSAT) umožňuje snadno vytvořit a spravovat uživatelské účty, definovat a použít role a navýšení kapacity na základě uživatelů a rolí autorizační pravidla pravopisu. WSAT ze sady Visual Studio můžete spustit tak, že přejdete do okna Průzkumník řešení a potom kliknete na ikonu konfigurace technologie ASP.NET nebo nabídky webu nebo projekt a pak vyberete požadovanou položku nabídky konfigurace technologie ASP.NET. Bohužel WSAT funguje jenom s místní weby. Proto nelze použít WSAT z pracovní stanice pro správu webu v produkčním prostředí.

Dobrou zprávou je, že všechny funkce vystavené poskytované WSAT jsou k dispozici prostřednictvím kódu programu přes rozhraní API rolí; a členství Kromě toho z obrazovky WSAT používat standardní ovládací prvky související s přihlášením technologie ASP.NET. Stručně řečeno můžete přidat stránky technologie ASP.NET na web, který nabízí možnosti správy potřeby.

Připomínáme, že starší kurz aktualizovali webovou aplikaci recenzí zahrnout `~/Admin` složky a tato složka není nakonfigurovaná a Povolit jenom uživatelé v roli správce. Po přidání na stránku k této složce s názvem `CreateAccount.aspx` z které správce může vytvořit nový uživatelský účet. Tato stránka používá ovládacím prvku CreateUserWizard zobrazit uživatelské rozhraní a back-end logiku pro vytvoření nového uživatelského účtu. Jaké další možnosti, s jsem si přizpůsobila tak ovládacího prvku zahrnout zaškrtávací políčko, zda se nový uživatel by měl být také přidáni do role správce (viz obrázek 5). Stačí nepatrné práce můžete vytvořit vlastní sadu stránek, který implementuje uživatelů a rolí souvisejících se správou úkolů, které by jinak poskytované WSAT.

> [!NOTE]
> Pro další informace o použití rozhraní API rolí a členství spolu s ovládacích prvků technologie ASP.NET související s přihlášením, nezapomeňte si přečíst Moje [ *kurzy o zabezpečení webu*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Další informace o přizpůsobení ovládacího prvku CreateUserWizard najdete [ *vytváření uživatelských účtů* ](../../older-versions-security/membership/creating-user-accounts-vb.md) a [ *ukládání Další informace o uživateli* ](../../older-versions-security/membership/storing-additional-user-information-vb.md) kurzy nebo rezervace [ *Erich Peterson* ](http://www.erichpeterson.com/) článku s [ *přizpůsobení ovládacího prvku CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Správci mohou vytvářet nové uživatelské účty](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Obrázek 5**: Správci mohou vytvářet nové uživatelské účty ([kliknutím ji zobrazíte obrázek v plné velikosti](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


Pokud je nutné úplné funkce aplikace WSAT rezervaci [ *se zajištěním provozu svůj vlastní nástroj pro správu webu*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), ve které autor Dan Clem vás provede procesem vytváření vlastního nástroje WSAT jako. DaN sdílené složky zdrojového kódu s jeho aplikace (v jazyce C#) a poskytuje podrobné pokyny pro přidání do vašeho prostředí webu.

## <a name="summary"></a>Souhrn

Při nasazení webové aplikace, která používá implementace databáze aplikace služby nutné nejdříve zkontrolovat, zda má databáze produkční požadavku databázových objektů. Tyto objekty lze přidat pomocí technik popsaných v *nasazení databáze* kurz; případně můžete použít `aspnet_regsql.exe` nástroj, jako jsme viděli v tomto kurzu. Další výzvy jsme nakousli center kolem název aplikace, které se používá ve vývojovém a produkčním prostředí (což je důležité, pokud chcete, aby uživatelé a role vytvořené ve vývojovém prostředí platný v produkčním prostředí) a techniky pro synchronizaci Správa uživatelů a rolí v provozním prostředí.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [*Nástroj pro registraci serveru SQL technologie ASP.NET (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Vytváří se databáze aplikace služby pro SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Vytvoření schématu členství v SQL serveru*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Zkoumání s členství technologie ASP.NET, role a profilu*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Vlastní nástroje pro správu webu se zajištěním provozu*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Kurzy zabezpečení webu*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Přehled nástroje pro správu webu*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Předchozí](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [další](strategies-for-database-development-and-deployment-vb.md)
