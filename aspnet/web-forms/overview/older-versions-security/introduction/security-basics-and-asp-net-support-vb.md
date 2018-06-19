---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Základy zabezpečení a podpora technologie ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Toto je první kurz v řadě návodů, které bude prozkoumat techniky pro ověřování návštěvníky prostřednictvím webového formuláře, autorizace přístupu k partic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: e62bb865e211a279b60f3120162ffc3c49cbdcc5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890005"
---
<a name="security-basics-and-aspnet-support-vb"></a>Základy zabezpečení a podpora technologie ASP.NET (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> Toto je první kurz v řadě návodů, které bude prozkoumat techniky pro návštěvníky prostřednictvím webového formuláře ověřování, autorizaci přístupu konkrétním stránek a funkce a správě uživatelských účtů v aplikaci ASP.NET.


## <a name="introduction"></a>Úvod

Co je jednou z věcí fóra, elektronické obchodování lokalit, online e-mailů webů, portálu webů a webů sociálních sítí, které všechny mají společnou? Všechny nabízejí *uživatelské účty*. Servery, které nabízejí uživatelské účty musí zadat počet služeb. Minimálně nové návštěvníci musí být schopen vytvořit účet a vracejících návštěvníci musí být moct přihlásit. Tyto webové aplikace můžete rozhodnout na základě přihlášeného uživatele: některé stránky nebo akce, může být omezen na jen zaznamená do protokolu v uživatele nebo podmnožinu uživatelů; Další stránky může zobrazovat informace specifické pro přihlášeného uživatele nebo může být vyšší nebo nižší zobrazují informace, v závislosti na tom, co uživatel prohlíží stránky.

Toto je první kurz v řadě návodů, které bude prozkoumat techniky pro návštěvníky prostřednictvím webového formuláře ověřování, autorizaci přístupu konkrétním stránek a funkce a správě uživatelských účtů v aplikaci ASP.NET. V průběhu následujících kurzech vyzkoušíme postup:

- Identifikovat a uživatele přihlašovat k webu
- Pomocí prostředí ASP. Na NET framework členství pro správu uživatelských účtů
- Vytvářet, aktualizovat a odstraňovat uživatelské účty
- Omezit přístup k webové stránce, adresáře nebo určitých funkcí na základě přihlášeného uživatele
- Pomocí prostředí ASP. Na NET framework role pro přidružení uživatelské účty s rolemi
- Spravovat uživatelské role
- Omezit přístup k webové stránce, adresáře nebo určitých funkcí na základě role přihlášeného uživatele
- Přizpůsobit a rozšířit ASP. Ovládací prvky webového NET pro zabezpečení

Tyto kurzy jsou zaměřeny na se stručným a poskytují podrobné pokyny s dostatkem snímky obrazovky vizuálně provedou celým procesem. Každý kurzu je k dispozici v C# a Visual Basic verze a zahrnuje stahování kód dokončení použít. (Tento první kurz se zaměřuje na koncepty zabezpečení z hlediska vysoké úrovně a proto neobsahuje žádné přidružený kód.)

V tomto kurzu budeme zabývat koncepty důležité zabezpečení a jaká zařízení jsou k dispozici v technologii ASP.NET, který pomáhá při implementaci ověřování pomocí formulářů, ověřování, uživatelské účty a role. Můžeme začít!

> [!NOTE]
> Zabezpečení je důležitým aspektem jakékoli aplikace, která zahrnuje fyzické, technologické, a rozhodnutí, která a vyžaduje vysoký stupeň plánování a domény znalosti zásady. Tento kurz řady není určen jako vodítko pro vývoj zabezpečení webových aplikací. Místo toho zaměřuje konkrétně na ověřování pomocí formulářů, ověřování, uživatelské účty a rolí. Zatímco některé koncepty zabezpečení obkroužení vyřešit tyto problémy jsou popsané v této série, jiné jsou ponechána neprozkoumané.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Ověřování, autorizace, uživatelských účtů a rolí

Role ověřování, autorizace, uživatelské účty a jsou čtyři podmínky, které se budou používat velmi často v rámci tohoto kurzu řady tak chcete pozorně rychlé definovat tyto podmínky v kontextu zabezpečení webu. Ve model klient server, jako je Internet existuje mnoho scénářů, ve kterých je nutné k identifikaci klienta vytvoření požadavku. *Ověřování* je proces zjišťování identitu klienta. Je klient, který byl úspěšně identifikován se říká, že *ověřený*. Neidentifikovaný klienta se říká, že *neověřené* nebo *anonymní*.

Systémy zabezpečeného ověřování zahrnovat alespoň jeden z následujících tří omezující vlastnosti: [něco víte, něco máte nebo něco jsou](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Většina webových aplikací spoléhají na něco, co klient zná, například heslo nebo PIN kód. Informace, které slouží k identifikaci uživatelů - svoje uživatelské jméno a heslo, například - jsou označovány jako *pověření*. Tato řada kurz se zaměřuje na *ověřování pomocí formulářů*, což je model ověřování, kde uživatelé přihlásit k webu zadáním svých přihlašovacích údajů v podobě webové stránky. Jsme všechny došlo tento typ ověřování před. Přejděte na libovolný web elektronického obchodování. Až budete připraveni k rezervaci budete vyzváni k přihlášení zadáním uživatelského jména a hesla do textových polí na webové stránce.

Kromě identifikaci klientů, server chtít omezit, jaké prostředky nebo funkce jsou dostupné v závislosti na klientovi, vytvoření požadavku. *Autorizace* je proces pro určení toho, jestli konkrétní uživatel má oprávnění k přístupu k určitému prostředku nebo funkce.

A *uživatelský účet* je úložiště pro zachování informací o určitého uživatele. Uživatelské účty musí obsahovat minimálně informace, které jednoznačně identifikuje uživatele, jako je například přihlašovací jméno uživatele a heslo. Společně s tyto nezbytné informace může zahrnovat uživatelské účty věcmi, jako jsou: e-mailovou adresu uživatele; Datum a čas, kdy byl vytvořen účet; Datum a čas, kdy se poslední přihlášení; Křestní jméno a příjmení; telefonní číslo; a e-mailová adresa. Pokud používáte ověřování pomocí formulářů, informace o uživatelském účtu je obvykle uložen v relační databázi, jako je Microsoft SQL Server.

Webové aplikace, které podporují uživatelské účty může volitelně skupiny uživatelů do *role*. Role je jednoduše popisek, který se použije pro uživatele a poskytuje abstrakci pro definování autorizační pravidla a funkce na úrovni stránky. Web může obsahovat třeba roli správce pomocí autorizačních pravidel, které má každý, kdo ale správce pro přístup k určité sadě webových stránek. Navíc různé stránky, které jsou dostupné pro všechny uživatele (včetně jiným uživatelům než správcům) může zobrazit další data nebo nabízí další funkce po návštěvě uživatelů v roli správce. Použití rolí, jsme můžete definovat tyto autorizační pravidla na role role základ spíše než uživatel.

## <a name="authenticating-users-in-an-aspnet-application"></a>Ověřování uživatelů v aplikaci ASP.NET

Pokud uživatel zadá adresu URL do okna adresu svého prohlížeče nebo klikne na odkaz, díky prohlížeče [protokol HTTP (Hypertext Transfer)](http://en.wikipedia.org/wiki/HTTP) požadavek na webovém serveru pro zadaný obsah, je jej technologie ASP.NET stránky, image, JavaScript soubor, nebo jiný typ obsahu. Webový server za úkol vrácení požadovaný obsah. Při tom se musí určit několik věcí o požadavku, včetně, který vytvořil požadavek a jestli je identita oprávnění k načtení požadovaného obsahu.

Ve výchozím nastavení prohlížeče odesílají požadavky HTTP, které nemají žádné řazení identifikační informace. Ale pokud prohlížeč obsahovat informace o ověřování potom webový server spustí pracovní postup ověření, která se pokusí o identifikaci klienta vytvoření požadavku. Kroky pro ověřovací pracovní postup závisí na typu ověřování používá webové aplikace. Technologie ASP.NET podporuje tři typy ověřování: Windows, Passport a formuláře. Tato řada kurz se zaměřuje na ověřování pomocí formulářů, ale můžeme trvat několik minut a porovnání kontrastu úložiště uživatele ověřování systému Windows a pracovní postup.

### <a name="authentication-via-windows-authentication"></a>Ověřování pomocí ověřování systému Windows

Pracovní postup ověřování systému Windows používá jednu z následujících techniky ověřování:

- Základní ověřování
- Ověřování hodnotou hash
- Integrované ověřování systému Windows

Všechny tři techniky fungovat stejným způsobem, zhruba: když neoprávněným anonymní požadavek dorazí, webový server zašle zpět odpověď HTTP, která určuje, že autorizace je potřeba pokračovat. V prohlížeči pak zobrazí modální dialogové okno s výzvu k zadání uživatelského jména a hesla (viz obrázek 1). Tyto informace se pak odesílají zpět k webovému serveru přes záhlaví HTTP.


![Modální dialogové okno zobrazí výzvu k jeho přihlašovacích údajů](security-basics-and-asp-net-support-vb/_static/image1.png)

**Obrázek 1**: modální dialogové okno zobrazí výzvu k jeho přihlašovacích údajů


Zadaná pověření se ověřují proti úložiště uživatele Windows webový server. To znamená, že každý ověřeného uživatele ve vaší webové aplikaci, musí mít účet systému Windows ve vaší organizaci. To je běžné v intranetové scénáře. Ve skutečnosti při použití integrovaného ověřování systému Windows v prostředí intranetu, prohlížeč automaticky poskytuje webový server s přihlašovací údaje používané k přihlášení k síti, a tím potlačení dialogových oken na obrázku 1. Při ověřování systému Windows je skvělá pro aplikace v síti intranet, je obvykle pro webové aplikace nebude proveditelné vzhledem k tomu, že nechcete vytvořit účty v systému Windows pro každého uživatele, která se zaregistruje ve vaší lokalitě.

### <a name="authentication-via-forms-authentication"></a>Ověřování prostřednictvím ověřování pomocí formulářů

Ověřování pomocí formulářů, na druhé straně je ideální pro internetové webové aplikace. Odvolání, že ověřování pomocí formulářů rozpoznává uživatele tak, že vyzve uživatele k zadání přihlašovacích údajů prostřednictvím webového formuláře. V důsledku toho když uživatel se pokusí získat přístup k neoprávněným prostředku, jsou automaticky přesměrován na přihlašovací stránku, kde můžou zadat své přihlašovací údaje. Odeslaná přihlašovací údaje jsou pak ověřený proti úložiště vlastní uživatele – obvykle databáze.

Po ověření odeslaná přihlašovací údaje, *forms lístek ověřování* se vytvoří pro uživatele. Tento lístek označuje, že uživatel byl ověřen a obsahuje identifikační údaje, jako je například uživatelské jméno. Lístek pro ověřování pomocí formulářů je (obvykle) uložený jako soubor cookie na klientském počítači. Proto dalších návštěvách na web zahrnují lístek pro ověřování pomocí formulářů v požadavku HTTP, a umožňuje webové aplikace k identifikaci uživatele, jakmile se přihlásí.

Obrázek 2 ukazuje pracovní postup ověřování formulářů z vysoké úrovně vantage bodu. Všimněte si, jak částí ověřování a autorizace v technologii ASP.NET fungovat jako dva samostatné entity. Systém ověřování formulářů identifikuje uživatele (nebo hlásí, že jsou anonymní). Systém autorizace je to, co určuje, zda má uživatel přístup k požadovanému prostředku. Pokud uživatel není autorizovaný (jako jsou na obrázku 2 při pokusu o anonymně navštivte ProtectedPage.aspx), autorizace systému hlásí, že uživateli se odepře, způsobuje formuláře ověřování systému automaticky přesměruje uživatele na přihlašovací stránku.

Jakmile se uživatel úspěšně přihlášen, zahrnují následné žádosti HTTP lístek pro ověřování pomocí formulářů. Ověřování systému forms identifikuje jenom uživatel – je autorizace systému, který určuje, zda má uživatel přístup k požadovanému prostředku.


![Pracovní postup ověřování formulářů](security-basics-and-asp-net-support-vb/_static/image2.png)

**Obrázek 2**: pracovní postup ověřování formulářů


Jsme se proniknout do ověřování pomocí formulářů mnohem podrobněji v následujících dvou kurzech[Přehled ověřování založené na formulářích](an-overview-of-forms-authentication-vb.md) a [konfiguraci ověřování formulářů a rozšířené témata](forms-authentication-configuration-and-advanced-topics-vb.md). Další informace o ASP. Možnosti ověřování pro NET, najdete v části [ověřování pomocí technologie ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Omezení přístupu k webové stránky, adresářů a stránky funkce

Technologie ASP.NET obsahuje dva způsoby, jak určit, zda konkrétní uživatel má oprávnění pro přístup k konkrétní soubor nebo adresář:

- **Soubor autorizace** – od stránek ASP.NET a webových služeb jsou implementované jako soubory, které jsou umístěné na serveru webového systému souborů, přístup k těmto souborům lze zadat pomocí seznamů řízení přístupu (ACL). Soubor autorizace se nejčastěji používá pomocí ověřování systému Windows, protože seznamy ACL jsou oprávnění, které se vztahují na účty systému Windows. Pokud používáte ověřování pomocí formulářů, jsou všechny požadavky na úrovni systému operačního systému a soubor provedený stejný účet systému Windows, bez ohledu na uživatele, kteří navštěvují webu.
- **Autorizace adres URL**-vývojář stránky s autorizace adres URL, určuje autorizační pravidla v souboru Web.config. Tato pravidla autorizace zadejte co uživatelé nebo role mají povolen přístup nebo je odepřen přístup na určité stránky nebo adresářů v aplikaci.

Soubor ověřování a autorizace adres URL definujte autorizační pravidla pro přístup ke konkrétní stránky ASP.NET nebo pro všechny stránky ASP.NET v jednom adresáři. Pomocí těchto postupů jsme určit, aby ASP.NET odmítnout požadavky na konkrétní stránku pro konkrétní uživatele, nebo povolit přístup pro sadu uživatelů a odepřít přístup k ostatních uživatelů. Co o scénářích, kde všichni uživatelé přístup ke stránce, ale funkce stránky závisí na uživatele? Například mnoho webů, které podporují uživatelské účty mají stránky zobrazující jiný obsah nebo data pro ověřené uživatele a anonymní uživatelé. Anonymní uživatel může zobrazit odkaz pro přihlášení k webu, že ověřený uživatel bude místo toho zobrazí zpráva jako, Vítejte zpět, *uživatelské jméno* spolu s odkazem na odhlášení. Jiný příklad: při zobrazení položky na stránku aukce zobrazí různé informace v závislosti na tom, zda jste uchazeči nebo jeden dražeb položky.

Tyto úpravy na úrovni stránky, můžete to udělat deklarativně nebo prostřednictvím kódu programu. K zobrazení různých obsahu pro anonymní než ověřené uživatele, jednoduše přetažení [ovládací prvek LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) na stránku a zadejte odpovídající obsah do jeho AnonymousTemplate a LoggedInTemplate šablony. Alternativně můžete prostřednictvím kódu programu zjistíte, zda je aktuální požadavek ověřen, kdo je uživatel a jaké role náleží do (pokud existuje). Tyto informace můžete následně zobrazit nebo skrýt sloupce v mřížce nebo panelů na stránce.

Tato řada obsahuje tři návodů, které soustředit na autorizaci. ***Autorizace uživatele na základě***prozkoumá jak omezit přístup na stránku nebo stránky v adresáři pro konkrétní uživatelské účty; ***Autorizace podle role*** zjistí zadávání autorizační pravidla v roli úroveň; nakonec ***zobrazení obsahu na základě aktuálně zaznamenána v uživatele*** tento kurz popisuje úpravy konkrétní stránky obsahu a funkcí na základě uživatele, kteří navštěvují stránky. Další informace o ASP. Možnosti pro NET autorizace, najdete v části [autorizaci ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Uživatelské účty a rolí

SOUBOR ASP. Ověřování pomocí formulářů je NET poskytuje infrastrukturu pro uživatele k přihlášení k lokalitě a jejich ověření stavu zapamatovaných napříč návštěv stránky. A autorizace adres URL poskytuje rozhraní pro omezení přístupu ke konkrétní soubory nebo složky v aplikaci ASP.NET. Ani funkce, ale poskytuje možnosti pro ukládání informací o uživatelském účtu nebo Správa rolí.

Před aplikaci ASP.NET 2.0 se vývojáři slouží k vytváření vlastních úložiště uživatele a role. Byly také na hák pro návrh uživatelského rozhraní a psaní kódu pro základní uživatele souvisejícím s účtem stránky jako přihlašovací stránku a vytvořit nový účet, mimo jiné stránky. Bez žádné předdefinované uživatelské účet framework technologie ASP.NET, každý vývojář implementující uživatelské účty museli přicházejí na své vlastní rozhodnutí o návrhu na otázky, jako je, jak lze uložit hesel a dalších citlivých údajů? a co pokyny by měly I stanovit týkající se délku hesla a síly?

V současné době implementace uživatelské účty v aplikaci ASP.NET je mnohem jednodušší díky *členství framework* a integrované ovládací prvky webového přihlášení. Rozhraní členství je několik tříd v [obor názvů System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) , poskytovat funkce pro provádění úloh souvisejících s účtem základní uživatel. Třída klíče v rámci členství je [třída členství](https://msdn.microsoft.com/library/system.web.security.membership.aspx), která obsahuje metody, třeba:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Používá rozhraní členství [modelu poskytovatelů](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), který řádně odděluje členství rozhraní API z jeho implementace. To umožňuje vývojářům používat společné rozhraní API, ale umožňuje, aby uživatelé používali implementace, která vyhovuje potřebám vlastní svých aplikacích. Stručně řečeno třída členství definuje nezbytné funkce rozhraní (metody, vlastnosti a události), ale ve skutečnosti neposkytuje žádné podrobné informace o nasazení. Místo toho vyvolat metody třídy členství nakonfigurovaného zprostředkovatele, který je co provádí samotnou práci. Například když je volána metoda CreateUser třída členství, třída členství neví, podrobnosti o úložišti uživatele. Nebude vědět, pokud uživatelé se neudržují v databázi nebo v souboru XML nebo některé další úložiště. Třída členství prozkoumá určete zprostředkovatele delegovat volání konfigurace webové aplikace a třídy zprostředkovatele zodpovídá za ve skutečnosti vytvoření nového uživatelského účtu do úložiště příslušné uživatele. Tato interakce je znázorněno na obrázku 3.

Microsoft dodává dvě třídy zprostředkovatele členství v rozhraní .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementuje rozhraní API členství v servery služby Active Directory a Active Directory Application Mode (ADAM).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementuje rozhraní API členství v databázi systému SQL Server.

Tato řada kurz se zaměřuje výhradně na SqlMembershipProvider.


[![Zprostředkovatele modelu umožňuje různými implementacemi být bezproblémově zapojen do Framework](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Obrázek 03**: zprostředkovatele modelu umožňuje různými implementacemi být bezproblémově zapojen do Framework ([Kliknutím zobrazit obrázek v plné velikosti](security-basics-and-asp-net-support-vb/_static/image5.png))


Výhodou modelu poskytovatelů je, že alternativní implementace může být vyvinuty společností Microsoft, jiných dodavatelů nebo jednotlivé vývojáři a bezproblémově připojovány rozhraní členství. Například společnost Microsoft vydala [zprostředkovatel členství pro databáze aplikace Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Další informace o zprostředkovateli členství, najdete v části [zprostředkovatele Toolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx), což zahrnuje návod zprostředkovatele členství, ukázka vlastních poskytovatelů, více než 100 stránky dokumentace na zprostředkovatele modelu a dokončení zdrojového kódu pro předdefinované zprostředkovatele členství (konkrétně, ActiveDirectoryMembershipProvider a SqlMembershipProvider).

ASP.NET 2.0 zavedli taky rozhraní role. Jako rozhraní členství je rozhraní role vytvořené na modelu poskytovatelů. Jejího rozhraní API je vystaven prostřednictvím [role třídy](https://msdn.microsoft.com/library/system.web.security.roles.aspx) a rozhraní .NET Framework se dodává s tři třídy zprostředkovatele:

- [Zprostředkovatel AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -spravuje informace o rolích v úložišti zásad Správce autorizací, například služby Active Directory nebo ADAM.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -implementuje role v databázi systému SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) – přidruží informace o rolích na základě skupiny Windows návštěvníka. Tato metoda se obvykle používá s ověřováním systému Windows.

Tato řada kurz se zaměřuje výhradně na SqlRoleProvider.

Vzhledem k tomu, že zprostředkovatel modelu zahrnuje jediného rozhraní API dopředu směřujících (třídy členství a rolí), je možné vytvořit funkce kolem tohoto rozhraní API bez nutnosti starat o podrobnosti implementace – těch, které jsou zpracovávány vybrané stránkou zprostředkovatele Developer. Toto jednotné rozhraní API umožňuje společnosti Microsoft a jiných dodavatelů, jak sestavit ovládací prvky webového tohoto rozhraní s architektury členství a rolí. ASP.NET se dodává s řadou [ovládací prvky webového přihlášení](https://msdn.microsoft.com/library/ms178329.aspx) pro implementaci běžných uživatelská účet uživatelské rozhraní. Například [přihlášení řízení](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) vyzve uživatele k zadání přihlašovacích údajů, ověřuje je a pak je do protokolů prostřednictvím ověřování pomocí formulářů. [Ovládací prvek LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) nabízí šablon pro zobrazení různých značek pro anonymní uživatele a ověřené uživatele nebo různých značek na základě role uživatele. A [prvku CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) poskytuje podrobné uživatelské rozhraní pro vytvoření nového uživatelského účtu.

Pod pozadí různé ovládací prvky přihlášení komunikovat s architektury členství a rolí. Většina ovládacích prvků přihlášení se dá implementovat bez nutnosti napsat jediný řádek kódu. Tyto ovládací prvky podrobněji vyzkoušíme v budoucnu kurzy, včetně techniky pro rozšíření a přizpůsobení jejich funkce.

## <a name="summary"></a>Souhrn

Všechny webové aplikace, které podporují uživatelské účty vyžadují podobné funkce, jako například: možnosti pro uživatele k přihlášení a jejich protokolu stav zapamatovaných napříč návštěv stránky; webové stránky pro nové návštěvníky k vytvoření účtu; a schopnost vývojář stránky k určení, jaké prostředků, data a funkce jsou k dispozici co uživatelé nebo role. Úlohy, ověřování a autorizace uživatelů a správa uživatelských účtů a rolí je pozoruhodně snadné provést v aplikacích ASP.NET díky ověřování pomocí formulářů, autorizace adres URL a architektur členství a rolí.

V průběhu další kurzy několik vyzkoušíme tyto aspekty sestavením funkční webovou aplikaci od základů podrobné způsobem. V následujících dvou kurzu se podíváme na ověřování pomocí formulářů podrobně. Nemůžeme se zobrazí pracovního postupu ověřování formulářů v akci, dissect lístek pro ověřování pomocí formulářů, popisují otázky zabezpečení a zjistit, jak konfigurovat systém ověřování formulářů – všechny při vytváření webové aplikace, která umožňuje návštěvníkům přihlášení a odhlášení.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [ASP.NET 2.0 členství, role, ověřování pomocí formulářů a zdroje informací o zabezpečení](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [Pokyny pro technologii ASP.NET 2.0 zabezpečení](https://msdn.microsoft.com/library/ms998258.aspx)
- [Ověřování pomocí technologie ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Autorizaci ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Přehled ovládacích prvků ASP.NET přihlášení](https://msdn.microsoft.com/library/ms178329.aspx)
- [Zkoumání ASP.NET 2.0 je členství, role a profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Postupy: zabezpečení mé lokality pomocí členství a rolí?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Úvod do členství](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Středisko pro vývojáře MSDN zabezpečení](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 zabezpečení, členství a Role správy](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zprostředkovatel Toolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl tento kurz, který byl řady zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu zahrnují Alicja Maziarz, Jan Suru a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](forms-authentication-configuration-and-advanced-topics-cs.md)
> [další](an-overview-of-forms-authentication-vb.md)
