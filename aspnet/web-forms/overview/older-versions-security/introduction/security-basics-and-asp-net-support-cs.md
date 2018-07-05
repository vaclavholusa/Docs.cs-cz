---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: Základy zabezpečení a podpora ASP.NET (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Toto je první kurz série kurzů, které se probírají techniky pro ověřování návštěvníků prostřednictvím webového formuláře autorizaci přístupu k partic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: cbc2da996726c6e1d838624726e7323ba947ce1e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374847"
---
<a name="security-basics-and-aspnet-support-c"></a>Základy zabezpečení a podpora ASP.NET (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Toto je první kurz v sérii kurzů, které bude zkoumat techniky pro návštěvníky prostřednictvím webového formuláře pro ověřování, autorizaci přístupu ke konkrétní stránky a funkce a správa uživatelských účtů v aplikaci ASP.NET.


## <a name="introduction"></a>Úvod

Co je jedna věc fóra, webech pro elektronické obchodování, websites online e-mailu, portálu webů a weby sociálních sítí, které všechny mají společnou? Všechny tyto funkce nabízejí *uživatelské účty*. Servery, které nabízejí uživatelské účty, musíte zadat celou řadou služeb. Minimálně noví návštěvníci musí mít možnost vytvoření účtu služby a vracející návštěvníci musí být moct přihlásit. Tyto webové aplikace můžete rozhodovat na základě přihlášeného uživatele: některé stránky nebo akce může být omezena na jen pro přihlášení uživatele nebo podmnožinu uživatelů. Další stránky může zobrazovat informace specifické pro přihlášeného uživatele, nebo možná viděli víc nebo míň informací, v závislosti na tom, co uživatel je zobrazení stránky.

Toto je první kurz v sérii kurzů, které bude zkoumat techniky pro návštěvníky prostřednictvím webového formuláře pro ověřování, autorizaci přístupu ke konkrétní stránky a funkce a správa uživatelských účtů v aplikaci ASP.NET. V průběhu následujících kurzech bude Zkoumáme, jak:

- Identifikovat a přihlašování uživatelů v na web
- Použití prostředí ASP. Na .NET framework členství pro správu uživatelských účtů
- Vytvářet, aktualizovat a odstraňovat uživatelské účty
- Omezit přístup k webové stránce, adresáře nebo určitých funkcí na základě přihlášeného uživatele
- Použití prostředí ASP. Na .NET framework role přiřadit uživatelské účty s rolemi
- Správa rolí uživatelů
- Omezit přístup k webové stránce, adresáře nebo určitých funkcí na základě role přihlášeného uživatele
- Přizpůsobení a rozšíření prostředí ASP. NET pro zabezpečení webové ovládací prvky

Tyto kurzy jsou zaměřené na být stručné a poskytují podrobné pokyny s dostatečným snímky obrazovky, který vás provede procesem vizuálně. Každý kurz je dostupná v C# a Visual Basic verze a nabízí ke stažení kompletní kód používá. (Tento první kurz se zaměřuje na koncepty zabezpečení z hlediska vysoké úrovně a proto neobsahuje žádný přidružený kód.)

V tomto kurzu se budeme zabývat koncepty důležité zabezpečení a jaká zařízení jsou k dispozici v technologii ASP.NET, které pomáhají při implementaci ověřování pomocí formulářů, autorizace, uživatelských účtů a rolí. Pusťme se do práce!

> [!NOTE]
> Zabezpečení je důležitou součástí jakékoli aplikace, která zahrnuje fyzického, technologického a zásady rozhodnutí a vyžaduje vysoký stupeň znalosti plánování a domény. V této sérii kurzů není určen jako vodítko pro vývoj zabezpečené webové aplikace. Místo toho zaměřuje konkrétně na ověřování pomocí formulářů, autorizace, uživatelských účtů a rolí. Zatímco některé koncepty zabezpečení obkroužení vyřešit tyto problémy jsou popsány v této sérii, ostatní jsou ponechána neprozkoumaných.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Ověřování, autorizace, uživatelských účtů a rolí

Ověřování, autorizace, uživatelských účtů a rolí jsou čtyři podmínky, které se použijí velmi často v celé této sérii, takže chci rychle využít k definování těchto podmínek v rámci kontextu zabezpečení webového. V modelu klient server, jako je například Internet existuje mnoho scénářů, ve kterých je potřeba na server identifikovat klientovi provádějícímu žádost. *Ověřování* je proces zjišťování identity klienta. Je klient, který se úspěšně identifikovala se říká, že *ověření*. Neidentifikovaný klienta se říká, že *neověřené* nebo *anonymní*.

Systémy zabezpečené ověřování zahrnovat alespoň jeden z následujících tří omezující vlastnosti: [něco vědět, něco, co máte nebo něco jsou](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Většina webových aplikací spoléhá na něco, co klient zná, například heslo nebo PIN kód. Informace, které slouží k identifikaci uživatelů - své uživatelské jméno a heslo, například - jsou označovány jako *pověření*. V této sérii kurzů se zaměřuje na *ověřování pomocí formulářů*, což je model ověřování, kde uživatelé přihlásit k webu pomocí svých přihlašovacích údajů ve formě webové stránky. Všechny se kterými jsme se tento typ ověřování než. Přejdete na libovolný web elektronického obchodování. Až budete připravení k rezervaci budete vyzváni k přihlášení tak, že zadáte svoje uživatelské jméno a heslo do textových polí na webové stránce.

Kromě identifikace klientů, server bude pravděpodobně vyžadovat omezit, jaké prostředky nebo funkcích jsou k dispozici v závislosti na klientovi provádějícímu žádost. *Autorizace* je proces určující, zda má konkrétní uživatel oprávnění pro přístup ke konkrétnímu prostředku nebo funkce.

A *uživatelský účet* je úložiště pro uchování informací o konkrétním uživateli. Uživatelské účty musí obsahovat minimálně informace, které jednoznačně identifikuje uživatele, například uživatele přihlašovací jméno a heslo. Společně tyto základní informace, mohou zahrnovat uživatelské účty věci, jako je: e-mailovou adresu uživatele; Datum a čas, kdy byl vytvořen účet; Datum a čas poslední přihlásili; Křestní jméno a příjmení; telefonní číslo. a poštovní adresu. Pokud používáte ověřování pomocí formulářů, informace o uživatelském účtu jsou obvykle uložená v relační databázi, jako je Microsoft SQL Server.

Webové aplikace, které podporují uživatelské účty mohou volitelně skupině uživatelů do *role*. Role je jednoduše popisek, který se použije pro uživatele a poskytuje abstrakci pro definování autorizační pravidla a funkce na úrovni stránky. Web může obsahovat třeba roli správce pomocí autorizačních pravidel, které zakazují, aby se nikdo správce pro přístup k určité sadě webových stránek. Navíc různé stránky, které jsou dostupné pro všechny uživatele (včetně bez oprávnění správce) může zobrazit další data nebo nabízejí doplňující funkce při návštěvě uživatelů v roli správce. Použití rolí, jsme na základ role pomocí role spíše než uživatele podle uživatelského definovat tyto autorizačních pravidel.

## <a name="authenticating-users-in-an-aspnet-application"></a>Ověřování uživatelů v aplikaci ASP.NET

Když uživatel zadá adresu URL do okna adresy jejich prohlížeče nebo kliknutí na odkaz, provede v prohlížeči [protokol HTTP (Hypertext Transfer)](http://en.wikipedia.org/wiki/HTTP) požadavku na webový server pro zadaný obsah, ať už jde ASP.NET stránce, obrázek, JavaScriptu soubor, nebo jakýkoli jiný typ obsahu. Webový server je za úkol vrací požadovaný obsah. Přitom ho musíte určit řadu věcí o žádosti, včetně, který vytvořil požadavek a určuje, zda je identita autorizované k získání požadovaného obsahu.

Ve výchozím nastavení prohlížeče odesílají požadavky HTTP, které nemají žádné řazení identifikační informace. Ale pokud prohlížeč neobsahuje informace o ověřování webový server spustí pracovní postup ověření, která se pokusí identifikovat klientovi provádějícímu žádost. Kroky pro ověřovací pracovní postup závisí na typu ověřování používá webové aplikace. Technologie ASP.NET podporuje tři typy ověřování: Windows, služby Passport a formuláře. V této sérii kurzů se zaměřuje na ověřování pomocí formulářů, ale pojďme si krátce můžete porovnat a kontrastu úložiště uživatele ověřování Windows a pracovního postupu.

### <a name="authentication-via-windows-authentication"></a>Ověření prostřednictvím ověřování Windows

Ověřovací pracovní postup Windows používá jednu z následujících postupů ověřování:

- Základní ověřování
- Ověřování algoritmem Digest
- Integrované ověřování Windows

Všechny tři techniky fungují přibližně stejně: když neoprávněným, anonymní žádosti o přijetí, webový server odešle zpět odpověď HTTP, která označuje, že autorizace je potřeba pokračovat. Prohlížeč zobrazí modální dialogové okno, které výzvu k zadání uživatelského jména a hesla (viz obrázek 1). Tyto informace se pak posílají zpět do webového serveru pomocí hlavičky protokolu HTTP.


![Modální dialogové okno se zobrazí výzva k jeho přihlašovacím údajům](security-basics-and-asp-net-support-cs/_static/image1.png)

**Obrázek 1**: modální dialogové okno se zobrazí výzva k jeho přihlašovacím údajům


Zadané přihlašovací údaje se ověří proti Store uživatelské web server Windows. To znamená, že každý ověřený uživatel ve vaší webové aplikace musí mít účet Windows ve vaší organizaci. To je běžné ve scénářích s intranetem. Ve skutečnosti při používání integrovaného ověřování Windows v prostředí intranetu, prohlížeč automaticky poskytne webový server pověření používaná pro přihlášení k síti, a tím potlačení dialogových oken na obrázku 1. Ověřování Windows jsou skvělé pro intranetové aplikace, je obvykle bylo neproveditelné pro aplikace, vzhledem k tomu, že nechcete vytvářet účty Windows pro každého uživatele, která se zaregistruje ve vaší lokalitě.

### <a name="authentication-via-forms-authentication"></a>Ověření prostřednictvím ověřování pomocí formulářů

Ověřování pomocí formulářů, na druhé straně je ideální pro webové aplikace. Vzpomínáte, že ověřování pomocí formulářů rozpoznává uživatele tak, že zobrazení výzvy k zadání přihlašovacích údajů prostřednictvím webového formuláře. V důsledku toho když se uživatel pokusí získat přístup k neoprávněným prostředku, je automaticky přesměrován na přihlašovací stránku, kde můžou zadat své přihlašovací údaje. Zadané přihlašovací údaje se pak ověří proti úložiště vlastního uživatele – obvykle databáze.

Po ověření zadané přihlašovací údaje, *forms ověřovací lístek* se vytvoří pro uživatele. Tento lístek označuje, že uživatel byl ověřen a obsahuje identifikační údaje, jako je například uživatelské jméno. Lístek ověřování pomocí formulářů (obvykle) uložený jako soubor cookie na klientském počítači. Proto následné návštěvy webu zahrnout ověřovací lístek požadavku HTTP, a tím umožnit webové aplikace k identifikaci uživatele, jakmile se přihlásíte.

Obrázek 2 ukazuje pracovní postup ověřování formulářů z vyšší úrovně vantage bodu. Všimněte si, jak kusy ověřování a autorizace v technologii ASP.NET fungovat jako dva samostatné entity. Systém ověřování formulářů identifikuje uživatele (nebo oznámí, že jsou anonymní). Co určuje, zda má uživatel přístup k požadovanému prostředku je systém autorizace. Pokud uživatel nemá oprávnění (jako jsou na obrázku 2, při pokusu o anonymně navštivte ProtectedPage.aspx), systém autorizace ohlásí, že uživateli je zakázán, způsobí formuláře ověřovacího systému automaticky přesměrovat uživatele na přihlašovací stránku.

Po úspěšném přihlášení uživatele patří následné žádosti HTTP lístek ověřování pomocí formulářů. Systém ověřování formulářů identifikuje pouze uživatele – je systém autorizace, která určuje, zda má uživatel přístup k požadovanému prostředku.


![Ověřovací pracovní postup formulářů](security-basics-and-asp-net-support-cs/_static/image2.png)

**Obrázek 2**: pracovní postup ověřování formulářů


Budou věnovat podrobněji ověřování pomocí formulářů mnohem podrobněji v následujících dvou kurzech[Přehled ověřování založené na formulářích](an-overview-of-forms-authentication-cs.md) a [konfigurace ověřování formulářů a témata pokročilé](forms-authentication-configuration-and-advanced-topics-cs.md). Další informace o ASP. Možnosti ověřování pro sítě, naleznete v tématu [ověřování ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Omezení přístupu k webové stránky, adresářů a funkce stránky

Technologie ASP.NET obsahuje dva způsoby, jak určit, jestli konkrétní uživatel má oprávnění pro přístup k konkrétní soubor nebo adresář:

- **Autorizace souboru** – protože stránek ASP.NET a webových služeb jsou implementovány jako soubory, které se nacházejí v systému souborů webového serveru, přístup k těmto souborům se dá nastavit pomocí seznamů řízení přístupu (ACL). Autorizace souboru se nejčastěji používá s ověřováním Windows, protože seznamy ACL jsou oprávnění, které se vztahují na účty Windows. Při použití ověřování pomocí formulářů, všechny požadavky operačního systému a soubor úrovni systému jsou spouštěny příkazem stejný účet Windows, bez ohledu na následujícím webu uživatele.
- **Autorizace adres URL**– s autorizace adres URL, vývojář stránky určuje autorizační pravidla v souboru Web.config. Tato pravidla autorizace zadejte co uživatele nebo role mají povolen přístup nebo byl odepřen přístup k určitým stránky nebo adresářů v aplikaci.

Soubor ověřování a autorizace adres URL definovat autorizační pravidla pro přístup ke konkrétní stránce technologie ASP.NET nebo pro všechny stránky technologie ASP.NET v určitém adresáři. Pomocí následujících postupů nám můžete dát pokyn ASP.NET odepřít požadavky na konkrétní stránce pro konkrétního uživatele nebo povolit přístup pro skupinu uživatelů a odepřít přístup kdokoli jiný. Co o scénářích, kde všichni uživatelé přístup ke stránce, ale na stránce funkce závisí na uživatele? Například mnoho serverů, které podporují uživatelské účty mají stránky, které se zobrazují jiným obsahem nebo data ověřených uživatelů a počet anonymních uživatelů. Anonymní uživatel může zobrazit odkaz pro přihlášení k webu, zatímco ověřeného uživatele by místo toho zobrazí zpráva, například, Vítejte zpět, *uživatelské jméno* spolu s odkazem na odhlášení. Další příklad: při zobrazení položky na webu aukce by se zobrazit různé informace v závislosti na tom, jestli jste jednu dražeb položky nebo uchazeči.

Tyto úpravy na úrovni stránky, můžete to udělat pomocí deklarace nebo prostřednictvím kódu programu. K zobrazení pro jiný obsah než ověřené uživatele, přetáhněte jednoduše anonymní [ovládacího prvku LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) na stránku a do jeho AnonymousTemplate a LoggedInTemplate šablony zadejte příslušný obsah. Alternativně můžete programově určit, zda je aktuální požadavek ověřen, kdo tento uživatel je a jaké role patří (pokud existuje). Tyto informace můžete poté zobrazit nebo skrýt sloupce v mřížce nebo panelů na stránce.

Tato řada obsahuje tři kurzy, které se soustředí na autorizaci. ***Autorizace na základě uživatele***zkoumá, jak omezit přístup na stránku nebo stránky v adresáři pro konkrétní uživatelské účty; ***Autorizační role na základě*** zjistí úroveň; a konečně, poskytnutí autorizační pravidla v roli ***zobrazení obsahu na základě právě protokolovány v uživatele*** tento kurz popisuje úpravy konkrétní na stránce obsah a funkce na základě uživatele na stránce. Další informace o ASP. Možnosti autorizace vaší sítě, naleznete v tématu [autorizaci ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx).


## <a name="user-accounts-and-roles"></a>Uživatelské účty a role

ASP. Ověřování pomocí formulářů pro síť slouží jako infrastruktura pro uživatele k přihlášení k webu a jejich ověření stavu zapamatovaných napříč návštěv stránky. A autorizace adres URL poskytuje rozhraní pro omezení přístupu ke konkrétní soubory nebo složky v aplikaci technologie ASP.NET. Žádná funkce, ale poskytuje prostředky pro ukládání informací o uživatelském účtu a správu role.

Před ASP.NET 2.0 tým byl zodpovědný za vytváření své vlastní úložiště pro uživatele a roli vývojáře. Byly také na hook pro návrh uživatelského rozhraní a psaní kódu pro základní uživatele souvisejícím s účtem stránky jako přihlašovací stránky a stránky a vytvořte nový účet, mimo jiné. Bez jakýkoli předdefinované uživatelské rozhraní účtu v technologii ASP.NET, každý vývojář měl implementující uživatelské účty můžete přejít na vlastní rozhodnutí o návrhu na otázky typu, jak můžu ukládat hesla a dalších citlivých údajů? a co pokyny by měly I stanovit týkající se délka hesla a šifrování?

V současné době provádění uživatelské účty v aplikaci technologie ASP.NET je mnohem jednodušší díky *členství v rámci* a integrovaného přihlášení webové ovládací prvky. Členství v rámci je několik tříd v [obor názvů System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) , které poskytují funkce pro provádění úlohy související s účtem základní uživatele. Třída klíče v rámci členství je [třída členství](https://msdn.microsoft.com/library/system.web.security.membership.aspx), který má metody, jako je:

- Metody CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Používá rozhraní členství [modelu poskytovatele](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), který čistě odděluje členství v rámci rozhraní API od jeho implementace. To umožňuje vývojářům používat společné rozhraní API, ale umožňuje jejich použití, která vyhovuje potřebám vaší aplikace vlastní implementaci. Stručně řečeno třída členství definuje základních funkcí rozhraní (metody, vlastnosti a události), ale ve skutečnosti neposkytuje žádné podrobnosti implementace. Místo toho vyvolat metody třídy členství nakonfigurovaného poskytovatele, který je co provádí samotnou práci. Například při vyvolání metody CreateUser třídy členství třída členství neví, podrobnosti o úložišti uživatele. Nebude vědět, pokud uživatelé právě zachovány v databázi nebo v souboru XML nebo v některé další úložiště. Třída členství zkontroluje konfiguraci webové aplikace určit zprostředkovatele delegovat volání a třídy zprostředkovatele zodpovídá za vytvoření nového uživatelského účtu do úložiště příslušné uživatele. Tato interakce je znázorněné na obrázku 3.

Microsoft dodává se dvěma třídami zprostředkovatele členství v rozhraní .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) – implementuje rozhraní API pro členství v servery služby Active Directory a Active Directory Application Mode (ADAM).
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) – implementuje rozhraní API pro členství v databázi serveru SQL Server.

V této sérii kurzů se zaměřuje výhradně na SqlMembershipProvider.


[![Zprostředkovatel Model umožňuje různé implementace bude bez problémů zapojen do rozhraní Framework&lt;/ strong&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Obrázek 03**: poskytovatele Model umožňuje různé implementace bude bez problémů zapojen do rozhraní Framework ([kliknutím ji zobrazíte obrázek v plné velikosti](security-basics-and-asp-net-support-cs/_static/image5.png))


Výhodou podle modelu poskytovatele je, že můžete alternativní implementace vyvinutá společností Microsoft, externích dodavatelů nebo jednotlivé vývojáře a bezproblémově zapojí se do rozhraní členství. Například společnost Microsoft vydala [zprostředkovatel členství pro databáze aplikace Microsoft Access](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Další informace o zprostředkovateli členství, najdete [sada nástrojů pro poskytovatele](https://msdn.microsoft.com/asp.net/aa336558.aspx), která obsahuje návod zprostředkovateli členství, ukázka vlastních poskytovatelů, více než 100 stránky dokumentace na podle modelu poskytovatele a dokončení zdrojový kód pro předdefinované zprostředkovatele členství (konkrétně ActiveDirectoryMembershipProvider a SqlMembershipProvider).

ASP.NET 2.0 zavedli rozhraní role. Jako jsou členství v rámci rozhraní role propojitelnosti podle modelu poskytovatele. Své rozhraní API je zveřejněný prostřednictvím [role třídy](https://msdn.microsoft.com/library/system.web.security.roles.aspx) a rozhraní .NET Framework se dodává s tři třídy zprostředkovatele:

- [Zprostředkovatel AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) – spravuje informace o rolích v úložišti zásady Správce autorizací, jako je například služba Active Directory nebo ADAM.
- [Zprostředkovatele SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) – implementuje role v databázi serveru SQL Server.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) – přidruží informace o rolích na základě skupiny Windows návštěvníka. Tato metoda se obvykle používá s ověřováním Windows.

V této sérii kurzů se zaměřuje výhradně na zprostředkovatele SqlRoleProvider.

Protože podle modelu poskytovatele obsahuje jedno rozhraní API směřujícím (třídy členství a rolí), je možné vytvořit funkce kolem tohoto rozhraní API bez nutnosti starat o podrobnosti implementace – ty jsou zpracovávány vybrané stránkou zprostředkovatele pro vývojáře. Toto jednotné rozhraní API umožňuje Microsoftu a jiných dodavatelů k vytvoření ovládacích prvků webového rozhraní s architekturami, členství a rolí. Technologie ASP.NET se dodává s celou řadou [přihlášení webové ovládací prvky](https://msdn.microsoft.com/library/ms178329.aspx) pro implementaci běžných uživatelských rozhraní účtu uživatele. Například [ovládací prvek Login](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) vyzve uživatele k zadání přihlašovacích údajů, ověřuje je a protokoly je prostřednictvím ověřování pomocí formulářů. [Ovládacího prvku LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) nabízí šablony pro zobrazení různých značek pro anonymní uživatele a ověření uživatelé nebo jiné značky na základě role uživatele. A [prvku CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) poskytuje podrobné uživatelské rozhraní pro vytvoření nového uživatelského účtu.

Pod pokličkou různých ovládacími prvky pro přihlášení interakci s architekturami, členství a rolí. Je možné implementovat většina ovládacími prvky pro přihlášení, aniž byste museli napsat jediný řádek kódu. Tyto ovládací prvky podrobněji prozkoumáme v budoucích kurzech, včetně postupů pro rozšíření a přizpůsobení jejich funkce.

## <a name="summary"></a>Souhrn

Všechny webové aplikace, které podporují uživatelské účty vyžadují podobné funkce, jako například: možnost uživatelů přihlásit a jejich protokolu ve stavu zapamatovaných napříč návštěv stránky; Webová stránka pro noví návštěvníci, chcete-li vytvořit účet. a možnost vývojář stránky k určení, jaký prostředek, data a funkce jsou k dispozici co uživatelů nebo rolí. Úlohy ověřování a autorizace uživatelů a správa uživatelských účtů a rolí je výjimečně snadné v aplikacích ASP.NET díky ověřování pomocí formulářů, autorizace adres URL a architektur členství a rolí.

V průběhu několika další kurzy prozkoumáme tyto aspekty sestavením funkční webovou aplikaci od základů podrobné způsobem. V následujících dvou kurzu se podíváme podrobněji ověřování pomocí formulářů. Uvidíme pracovní postup ověřování formulářů v akci, dissect lístek ověřování pomocí formulářů, popisují otázky zabezpečení a zjistit, jak nakonfigurovat systém ověřování formulářů – při vytváření webové aplikace, která umožňuje návštěvníkům přihlášení a odhlášení.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [ASP.NET 2.0 členství, role, ověřování pomocí formulářů a zabezpečení prostředků](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0 bezpečnostní pokyny](https://msdn.microsoft.com/library/ms998258.aspx)
- [Ověřování pomocí technologie ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [Povolení technologie ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Přehled ovládacích prvků technologie ASP.NET pro přihlášení](https://msdn.microsoft.com/library/ms178329.aspx)
- [Zkoumání ASP.NET 2.0 pro členství, role a profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Jak mohu: zabezpečení webu pomocí členství a rolí?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Úvod do členství](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [Centrum zabezpečení pro vývojáře MSDN](https://msdn.microsoft.com/security/default.aspx)
- [Profesionální ASP.NET 2.0 zabezpečení, členství a rolí správy](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Sada nástrojů pro poskytovatele](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu se v tomto kurzu, který řady byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu zahrnují Alicja Maziarz, Jan Suru a Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](an-overview-of-forms-authentication-cs.md)
