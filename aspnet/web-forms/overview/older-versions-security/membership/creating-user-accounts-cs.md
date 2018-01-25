---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
title: "Vytváření uživatelských účtů (C#) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu se podíváme na použití rozhraní členství (prostřednictvím SqlMembershipProvider) k vytvoření nových uživatelských účtů. Zjistíme, jak vytvořit nový nám..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: f175278c-6079-4d91-b9b4-2493ed43d9ec
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: d1bdec096b68a01c36f46765abef00aad319f2c2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="creating-user-accounts-c"></a>Vytváření uživatelských účtů (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_cs.pdf)

> V tomto kurzu se podíváme na použití rozhraní členství (prostřednictvím SqlMembershipProvider) k vytvoření nových uživatelských účtů. Postup vytvoření nové uživatele prostřednictvím kódu programu a prostřednictvím ASP jsme se zobrazí. NET na integrované CreateUserWizard řízení.


## <a name="introduction"></a>Úvod

V <a id="_msoanchor_1"> </a> [předchozí kurzu](creating-the-membership-schema-in-sql-server-cs.md) jsme nainstalovali schématu aplikací služby v databázi, která přidání tabulek, zobrazení a uložené procedury vyžaduje `SqlMembershipProvider` a `SqlRoleProvider`. To vytvoří infrastrukturu, je nutné zadat pro zbývající z této série kurzů. V tomto kurzu se podíváme na použití rozhraní členství (prostřednictvím `SqlMembershipProvider`) k vytvoření nových uživatelských účtů. Postup vytvoření nové uživatele prostřednictvím kódu programu a prostřednictvím ASP jsme se zobrazí. NET na integrované CreateUserWizard řízení.

Kromě naučit, jak vytváření nových uživatelských účtů, bude také je potřeba aktualizovat ukázkový web jsme vytvořili nejprve v  *<a id="_msoanchor_2"> </a> [Přehled ověřování založené na formulářích](../introduction/an-overview-of-forms-authentication-cs.md)*  kurz a pak v rozšířené  *<a id="https://www.asp.net/learn/security/tutorial-03-cs.aspx"> </a> konfiguraci ověřování formulářů a rozšířené témata* kurzu. Naše ukázkové webové aplikace má přihlašovací stránku, která ověřuje přihlašovací údaje uživatelů s páry pevně uživatelského jména a hesla. Kromě toho `Global.asax` zahrnuje kód, který vytvoří vlastní `IPrincipal` a `IIdentity` objekty pro ověřené uživatele. Budeme aktualizovat přihlašovací stránku k ověření pověření uživatele pro rozhraní členství a odebrat vlastní logiky zabezpečení a identity.

Můžeme začít!

## <a name="the-forms-authentication-and-membership-checklist"></a>Ověřování pomocí formulářů a kontrolní seznam členství

Než začneme práci s rozhraní členství, Podívejme se na chvíli ke kontrole důležité kroky, které jsme provedli k dosažení tohoto bodu. Pokud používáte framework členství s `SqlMembershipProvider` ve scénáři ověřování pomocí formulářů, musí být provedeny před jejich implementací funkce členství ve webové aplikaci následující kroky:

1. **Povolte ověřování pomocí formulářů.** Jak již bylo zmíněno  *<a id="_msoanchor_4"> </a> [Přehled ověřování založené na formulářích](../introduction/an-overview-of-forms-authentication-cs.md)*, ověřování pomocí formulářů je povoleno úpravou `Web.config` a nastavení `<authentication>` elementu `mode` atribut `Forms`. S povolit ověřování pomocí formulářů, je zkontrolován každého příchozího požadavku pro *forms lístek ověřování*, která, pokud existuje, identifikuje žadatel.
2. **Přidejte služby schématu aplikací k příslušné databázi.** Při použití `SqlMembershipProvider` potřebujeme nainstalovat schématu aplikací služby k databázi. Obvykle toto schéma se přidá do stejné databáze, která obsahuje model dat aplikace. *<a id="_msoanchor_5"> </a> [Vytváření schématu členství v systému SQL Server](creating-the-membership-schema-in-sql-server-cs.md)*  kurzu hledá pomocí `aspnet_regsql.exe` nástroj toho chcete dosáhnout.
3. **Upravte nastavení webové aplikace tak, aby odkazovaly databázi z kroku 2.** *Vytváření schématu členství v systému SQL Server* kurz vám ukázal, dva způsoby konfigurace webové aplikace tak, aby `SqlMembershipProvider` by použít databázi vybrané v kroku 2: změnou `LocalSqlServer` název připojovacího řetězce; nebo přidáním nového registrované zprostředkovatele do seznamu zprostředkovatelů členství framework a přizpůsobení tohoto nového poskytovatele chcete použít databázi z kroku 2.

Při vytváření webové aplikace, které používá `SqlMembershipProvider` a ověřování pomocí formulářů, budete muset provést tyto tři kroky před použitím `Membership` třídu nebo ovládací prvky ASP.NET – webové přihlášení. Vzhledem k tomu, že jsme již provést tyto kroky v předchozí kurzy, jsme připraveni začít používat rozhraní členství!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Přidání nových stránek ASP.NET

V tomto kurzu a další tři jsme se prověřují různých funkcí spojených se členství a možnosti. Potřebujeme řadu stránek ASP.NET implementovat témata zkontrolován v rámci těchto kurzů. Umožňuje vytvořit tyto stránek a pak soubor mapy webu `(Web.sitemap)`.

Začněte vytvořením novou složku v projektu s názvem `Membership`. Dál přidejte pět nových stránek ASP.NET do `Membership` složky, propojení každou stránku s `Site.master` stránky předlohy. Název stránky:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Vašeho projektu Průzkumníku řešení v tomto okamžiku by mělo vypadat jako na uvedené na obrázku 1 snímku obrazovky.


[![Pět nových stránek byly přidány do složky členství](creating-user-accounts-cs/_static/image2.png)](creating-user-accounts-cs/_static/image1.png)

**Obrázek 1**: pět nové stránky přidané `Membership` složky ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image3.png))


Každé stránce měli v tomto okamžiku dva ovládací prvky obsahu, jeden pro každou stránku předlohy ContentPlaceHolders: `MainContent` a `LoginContent`.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample1.aspx)]

Odvolat, který `LoginContent` ContentPlaceHolder na výchozí značka zobrazí odkaz na přihlášení a odhlášení lokality, v závislosti na tom, jestli je uživatel ověřený. Přítomnost `Content2` ovládací prvek obsahu, ale přepíše značek výchozí stránky předlohy. Jak již bylo zmíněno  *<a id="_msoanchor_6"> </a> [Přehled ověřování založené na formulářích](../introduction/an-overview-of-forms-authentication-cs.md)*  kurzu, to je užitečné v stránky, kde jsme nechcete zobrazit související s přihlášením možnosti v levém sloupci.

Těchto pět stránek, ale chceme zobrazit kód výchozí stránky předlohy pro `LoginContent` ContentPlaceHolder. Proto odebrat deklarativní `Content2` obsahu ovládacího prvku. Až to uděláte, každý z pěti stránce značek by měl obsahovat pouze jeden prvek obsahu.

## <a name="step-2-creating-the-site-map"></a>Krok 2: Vytvoření mapy webu

Všechny ale nejvíce trivial weby musí implementovat určitou formu navigační uživatelské rozhraní. Uživatelské rozhraní navigační může být jednoduchý seznam odkazů na různé části webu. Tyto odkazy můžete taky může být uspořádány do nabídky nebo zobrazení stromu. Jako vývojáři stránek vytvoření navigační uživatelské rozhraní je polovinu ze článku. Potřebujeme také některé znamená definovat strukturu logické lokality způsobem udržovatelný a lze aktualizovat. Jak se přidají nové stránky nebo odebrat existující stránky, chceme, abyste mohli aktualizovat jednoho zdroje – mapy webu – a mají tyto změny projeví v navigační uživatelské rozhraní webu.

Tyto dvě úlohy – definování mapy webu a implementace navigační uživatelské rozhraní na základě lokality mapy – se snadno plnit díky rozhraní mapy webu a ovládací prvky webového navigační přidají v technologii ASP.NET verze 2.0. Umožňuje rozhraní mapy webu pro vývojáře k definování mapy webu a potom k němu přístup prostřednictvím programovací rozhraní API ( [ `SiteMap` třída](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Zahrnout integrované ovládací prvky webové navigace [Menu – ovládací prvek](https://msdn.microsoft.com/library/bz09dy46.aspx), [TreeView – ovládací prvek](https://msdn.microsoft.com/library/3eafky27.aspx)a [prvek SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Jako rozhraní členství a rolí, je rozhraní mapy webu vytvořené na [zprostředkovatele modelu](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Úloha třídy zprostředkovatele mapy webu je k vytvoření struktury v paměti používané `SiteMap` třídy z trvalých dat úložiště, jako je soubor XML nebo databázové tabulky. Rozhraní .NET Framework se dodává s výchozí poskytovatel mapy webu, který čte data mapy webu ze souboru XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), a to je poskytovatel používáme v tomto kurzu. Některé alternativní implementace zprostředkovatele mapy webu naleznete v části Další odečty na konci tohoto kurzu.

Výchozí zprostředkovatel mapy webu očekává správně naformátován soubor XML s názvem `Web.sitemap` existovat kořenový adresář. Vzhledem k tomu, že používáme tento výchozího zprostředkovatele, je potřeba přidat takový soubor a definovat strukturu mapy webu v příslušném formátu XML. Chcete-li přidat soubor, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte Přidat novou položku. V dialogovém okně opt přidat soubor typu mapy webu s názvem `Web.sitemap`.


[![Přidejte soubor s názvem Web.sitemap do kořenového adresáře projektu](creating-user-accounts-cs/_static/image5.png)](creating-user-accounts-cs/_static/image4.png)

**Obrázek 2**: Přidat soubor s názvem `Web.sitemap` do kořenového adresáře projektu ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image6.png))


Soubor XML lokality mapy definuje strukturu webu jako hierarchie. Tento hierarchický vztah je modelován v souboru XML prostřednictvím dědičností z `<siteMapNode>` elementy. `Web.sitemap` Musí začínat `<siteMap>` nadřazeného uzlu, který obsahuje přesně jedno `<siteMapNode>` podřízené. Tento nejvyšší úrovně `<siteMapNode>` element reprezentuje kořen hierarchie a může mít libovolný počet podřízené uzly. Každý `<siteMapNode>` musí obsahovat element `title` atribut a může volitelně obsahovat `url` a `description` atributy, mimo jiné; každý neprázdný `url` atributu musí být jedinečný.

Zadejte následující kód XML do `Web.sitemap` souboru:

[!code-xml[Main](creating-user-accounts-cs/samples/sample2.xml)]

Výše uvedený kód lokality mapy definuje hierarchii znázorněný na obrázku 3.


[![Hierarchická struktura navigační představuje mapy webu](creating-user-accounts-cs/_static/image8.png)](creating-user-accounts-cs/_static/image7.png)

**Obrázek 3**: mapy webu představuje hierarchická struktura navigační ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Krok 3: Aktualizace stránky předlohy Zahrnout navigační uživatelské rozhraní

Technologie ASP.NET obsahuje řadu souvisejících s navigací webových ovládacích prvků pro návrh uživatelského rozhraní. Mezi ně patří, v nabídce, TreeView a ovládací prvky SiteMapPath. Nabídky a TreeView – ovládací prvky vykreslení struktury mapy webu v nabídce nebo strom, v uvedeném pořadí, zatímco SiteMapPath zobrazí zobrazení cesty, která ukazuje na aktuálním uzlu a také jeho předchůdců přístupu. Data mapy webu mohou být vázány na další data webové ovládací prvky pomocí SiteMapDataSource a je možné programově přistupovat prostřednictvím `SiteMap` třídy.

Vzhledem k tomu, že vyčerpávající diskusi o rozhraní mapy webu a ovládací prvky pro navigaci je nad rámec tohoto kurzu řad, spíš než věnovat času věnujte vlastní navigační uživatelské rozhraní umožňuje místo toho půjčit používaný v mé  *[ Práce s daty v technologii ASP.NET 2.0](../../data-access/index.md)*  kurz řady, která používá prvku Repeater pro zobrazení seznamu s odrážkami dva přímým navigační odkazy, jak je znázorněno na obrázku 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Přidání dvojúrovňové seznam odkazů v levém sloupci

Pokud chcete vytvořit toto rozhraní, přidejte následující deklarativní k `Site.master` stránky předlohy je levém sloupci kde text "TODO: nabídky bude přejděte sem..." nyní nachází.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample3.aspx)]

Výše uvedený kód vytvoří vazbu ovládacího prvku opakovače s názvem `menu` k SiteMapDataSource, která vrací definované v hierarchii mapy webu `Web.sitemap`. Protože ovládací prvek SiteMapDataSource [ `ShowStartingNode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) nastavena na hodnotu False, začne vrácení mapy webu hierarchie počínaje podřízeného prvku uzlu "Domů". Všechny tyto uzly (aktuálně pouze "členství") opakovače zobrazí v `<li>` elementu. Jiné, vnitřní opakovače pak zobrazí podřízené objekty aktuálního uzlu ve vnořených neuspořádaný seznam.

Na obrázku 4 je výše uvedený kód vygenerovaný výstup s struktury mapy webu, který jsme vytvořili v kroku 2. Opakovače vykreslí značku vanilla neuspořádaný seznam; pravidla šablony kaskádových definované v `Styles.css` jsou zodpovědní za-vkusnou rozložení. Podrobnější popis toho, jak kód výše funguje, najdete v části [hlavní stránky a webové navigace](https://asp.net/learn/data-access/tutorial-03-cs.aspx) kurzu.


[![Navigační uživatelské rozhraní je vykreslen použití vnořených neuspořádaný seznamů](creating-user-accounts-cs/_static/image11.png)](creating-user-accounts-cs/_static/image10.png)

**Obrázek 4**: navigační uživatelské rozhraní je vykreslen použití vnořených neuspořádaný seznamů ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Přidání navigace s popisem cesty

Kromě seznamu odkazů v levém sloupci, můžeme také mít každý zobrazení stránky [s popisem cesty](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Zobrazení cesty je element navigační uživatelského rozhraní, které rychle zobrazuje uživatele, kteří své aktuální pozici v hierarchii této lokality. Ovládací prvek SiteMapPath používá rozhraní mapy webu k určení aktuální stránky umístění v mapě webu a potom zobrazí zobrazení cesty na základě těchto informací.

Konkrétně přidat `<span>` element záhlaví stránky předlohy `<div>` elementu a nastavte nové `<span>` elementu `class` atribut "s popisem cesty". ( `Styles.css` Třída obsahuje pravidla pro třídu "s popisem cesty".) Dál přidejte SiteMapPath na tomto nové `<span>` elementu.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample4.aspx)]

Obrázek 5 ukazuje výstup SiteMapPath při návštěvě `~/Membership/CreatingUserAccounts.aspx`.


[![Zobrazí aktuální stránky s popisem cesty a namapujte jeho předchůdců v lokalitě](creating-user-accounts-cs/_static/image14.png)](creating-user-accounts-cs/_static/image13.png)

**Obrázek 5**: zobrazení cesty v mapy webu zobrazí aktuální stránce a jeho předchůdců ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Krok 4: Odebrání vlastní objekt zabezpečení a logiku Identity

V  *<a id="_msoanchor_7"> </a> [konfiguraci ověřování formulářů a rozšířené témata](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)*  kurzu jsme viděli postup přidružení vlastní objekty zabezpečení a identity k ověřenému uživateli. Jsme to vyřešit vytvořením obslužné rutiny události v `Global.asax` pro danou aplikaci `PostAuthenticateRequest` událost, která aktivuje se po `FormsAuthenticationModule` ověření uživatele. V této obslužné rutiny události jsme nahradit `GenericPrincipal` a `FormsIdentity` objektů přidaných `FormsAuthenticationModule` s `CustomPrincipal` a `CustomIdentity` objekty jsme vytvořili v tomto kurzu.

Při vlastní objekty zabezpečení a identity jsou užitečné v některých scénářích, ve většině případů `GenericPrincipal` a `FormsIdentity` postačí objekty. V důsledku toho je pravděpodobné, že je smysl se vraťte do výchozí chování. Tuto změnu provést odebráním nebo komentářů se `PostAuthenticateRequest` obslužné rutiny události nebo odstraněním `Global.asax` celý soubor.

## <a name="step-5-programmatically-creating-a-new-user"></a>Krok 5: Programové vytvoření nového uživatele

Chcete-li vytvořit nový uživatelský účet prostřednictvím použití framework členství `Membership` třídy [ `CreateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Tato metoda má vstupní parametry pro uživatelské jméno, heslo a další související s uživateli pole. Vyvolání, deleguje vytvoření nového uživatelského účtu do nakonfigurovaného zprostředkovatele členství a potom se vrátí [ `MembershipUser` objekt](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) představující vytvořeném uživatelský účet.

`CreateUser` Metoda má čtyři přetížení, každé přijímající jiný počet vstupních parametrů:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Tyto čtyři přetížení se liší na množství informací, které jsou shromážděny. První přetížení, například vyžaduje pouze uživatelské jméno a heslo pro nový uživatelský účet, zatímco druhý taky vyžaduje e-mailovou adresu uživatele.

Tato přetížení existovat, protože informace potřebné k vytvoření nového uživatelského účtu, závisí na nastavení konfigurace zprostředkovatele členství. V  *<a id="_msoanchor_8"> </a> [vytváření schématu členství v systému SQL Server](creating-the-membership-schema-in-sql-server-cs.md)*  kurzu jsme se zaměřili na zadání nastavení konfigurace pro zprostředkovatele členství v `Web.config`. Tabulka 2 zahrnuty úplný seznam nastavení konfigurace.

Jeden takový členství zprostředkovatele nastavení konfigurace, které ovlivňuje, co `CreateUser` přetížení lze je `requiresQuestionAndAnswer` nastavení. Pokud `requiresQuestionAndAnswer` je nastaven na `true` (výchozí), pak při vytváření nového uživatelského účtu jsme musíte zadat bezpečnostní otázku a odpověď. Tyto informace se později používá, když uživatel potřebuje můžete resetovat nebo změnit své heslo. Konkrétně v té době jsou uvedené na bezpečnostní otázku a jejich musíte zadat správný odpovědí, aby bylo možné resetovat nebo změnit své heslo. V důsledku toho pokud `requiresQuestionAndAnswer` je nastaven na `true` pak buď první dvě volání `CreateUser` přetížení má za následek výjimku, protože chybí bezpečnostní otázku a odpověď. Vzhledem k tomu, že naše aplikace je aktuálně konfigurována tak, aby vyžadovala bezpečnostní otázku a odpověď, budeme muset použijte jednu z druhé dvě přetížení při vytváření uživatele prostřednictvím kódu programu.

Pro ilustraci použití `CreateUser` metoda, můžeme vytvořit uživatelské rozhraní, ve kterých jsme požádat uživatele o jejich jméno, heslo, e-mailu a odpověď na předem definované bezpečnostní otázku. Otevřete `CreatingUserAccounts.aspx` stránku `Membership` složky a přidejte následující ovládací prvky webového do ovládacího prvku obsahu:

- Textové pole s názvem`Username`
- Textové pole s názvem `Password`, jejichž `TextMode` je nastavena na`Password`
- Textové pole s názvem`Email`
- Štítek s názvem `SecurityQuestion` s jeho `Text` vlastnost bylo vymazáno
- Textové pole s názvem`SecurityAnswer`
- Tlačítko s názvem `CreateAccountButton` jejíž Text je nastavena na "Vytvořit uživatelský účet"
- Ovládací prvek popisek s názvem `CreateAccountResults` s jeho `Text` vlastnost bylo vymazáno

Obrazovky v tomto okamžiku by mělo vypadat jako na uvedené na obrázku 6 snímku obrazovky.


[![Přidat na stránku CreatingUserAccounts.aspx různé ovládací prvky webového](creating-user-accounts-cs/_static/image17.png)](creating-user-accounts-cs/_static/image16.png)

**Obrázek 6**: přidat různé webové ovládací prvky `CreatingUserAccounts.aspx` stránky ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image18.png))


`SecurityQuestion` Popisek a `SecurityAnswer` textové pole, které jsou určeny k zobrazení předem definované bezpečnostní otázku a odpověď uživatele shromažďování. Všimněte si, že bezpečnostní otázku a odpověď jsou uložené na základě uživatel je tedy možné umožnit každému uživateli definovat vlastní bezpečnostní otázku. Pro tento příklad jste se rozhodli použít universal bezpečnostní otázku, a to ale: "Co je Oblíbené barevné?"

Chcete-li implementovat toto předem definované bezpečnostní otázku, přidejte do třídy kódu stránky s názvem konstanta `passwordQuestion`, jeho přiřazení na bezpečnostní otázku. Potom v `Page_Load` obslužné rutiny události, přiřaďte tuto konstanta, která se `SecurityQuestion` popisku `Text` vlastnost:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample5.cs)]

Dále vytvořte obslužnou rutinu události pro `CreateAccountButton`na `Click` událostí a přidejte následující kód:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample6.cs)]

`Click` Obslužné rutiny události spustí definováním proměnné s názvem `createStatus` typu [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus`je výčet, který označuje stav `CreateUser` operaci. Například, pokud uživatelský účet je úspěšně vytvořen, výsledná `MembershipCreateStatus` instance bude nastavena na hodnotu `Success`; na dalším ruční, pokud se operace nezdaří, protože už existuje uživatele se stejným uživatelským jménem, nastaví se na hodnotu `DuplicateUserName`. V `CreateUser` přetížení používáme, musíme předat `MembershipCreateStatus` instance do jako metodu `out` parametr. Tento parametr je nastaven na odpovídající hodnotu v rámci `CreateUser` metoda a My můžete zkontrolovat jeho hodnotu po volání metody k určení, zda uživatelský účet byl úspěšně vytvořen.

Po volání `CreateUser`a předejte `createStatus`, `switch` příkaz slouží k vypsání odpovídající zprávu v závislosti na hodnotu přiřazenou `createStatus`. Následující obrázky 7 zobrazuje výstup, když nového uživatele byla úspěšně vytvořena. Následující obrázky 8 a 9 zobrazit výstup při uživatelský účet není vytvořena. Na obrázku 8 návštěvníka zadali heslo pět písmenem, který nesplňuje požadavky na heslo sílu vyjádřeny v nastavení konfigurace zprostředkovatele členství. Na obrázku 9 se pokouší návštěvníka vytvořit uživatelský účet s existující uživatelské jméno (jeden, vytvořit na obrázku 7).


[![Nový uživatelský účet je úspěšně vytvořen](creating-user-accounts-cs/_static/image20.png)](creating-user-accounts-cs/_static/image19.png)

**Obrázek 7**: A nový uživatelský účet je úspěšně vytvořen ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image21.png))


[![Uživatelský účet nebyla vytvořena, protože zadané heslo je příliš slabé](creating-user-accounts-cs/_static/image23.png)](creating-user-accounts-cs/_static/image22.png)

**Obrázek 8**: uživatelský účet nebyla vytvořena, protože zadané heslo je příliš slabé ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image24.png))


[![Uživatelský účet je, že není vytvořen protože uživatelské jméno je již používán](creating-user-accounts-cs/_static/image26.png)](creating-user-accounts-cs/_static/image25.png)

**Obrázek 9**: uživatelský účet je vytvořen protože uživatelské jméno je již používán ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image27.png))


> [!NOTE]
> Asi vás zajímá, jak určit úspěch nebo neúspěch při pomocí jedné z první dvě `CreateUser` přetížení metody, ani z které má parametr typu `MembershipCreateStatus`. Tato první dva přetížení throw [ `MembershipCreateUserException` výjimka](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) při krátkodobém selhání, což zahrnuje [ `StatusCode` vlastnost](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) typu `MembershipCreateStatus`.


Po vytvoření jen několik uživatelských účtů, ověřte, že účty se vytvořila při výpisu obsahu `aspnet_Users` a `aspnet_Membership` tabulky v `SecurityTutorials.mdf` databáze. Jak ukazuje obrázek 10, I přidali dvě uživatele prostřednictvím `CreatingUserAccounts.aspx` stránky: Tito a Bruce.


[![Existují dva uživatelé v úložišti uživatele členství: Tito a Bruce](creating-user-accounts-cs/_static/image29.png)](creating-user-accounts-cs/_static/image28.png)

**Obrázek 10**: v úložišti uživatele členství jsou dva uživatelé: Tito a Bruce ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image30.png))


Zatímco úložiště uživatele členství teď obsahuje informace o Bruce a Tito na účtu, musíme ještě implementovat funkce, které umožňuje Bruce nebo Tito pro přihlášení k webu. V současné době `Login.aspx` ověřuje přihlašovací údaje uživatele s pevně zakódovaným sada párů uživatelského jména a hesla – nemá *není* ověřit zadaná pověření proti rozhraní členství. Pro nyní zobrazuje nové uživatelské účty v `aspnet_Users` a `aspnet_Membership` tabulky bude muset stačit. V dalším kurzu  *<a id="_msoanchor_9"> </a> [ověřování uživatele pověření proti členství uživatel uložit](validating-user-credentials-against-the-membership-user-store-cs.md)*, budeme aktualizovat přihlašovací stránku k ověření vůči úložišti členství.

> [!NOTE]
> Pokud nevidíte všechny uživatele ve vaší `SecurityTutorials.mdf` databázi, může to být proto vaší webové aplikaci používá výchozí zprostředkovatel členství `AspNetSqlMembershipProvider`, které používá `ASPNETDB.mdf` databáze jako své úložiště uživatele. Pokud chcete zjistit, pokud se jedná o problém, klikněte na tlačítko Aktualizovat v Průzkumníku řešení. Pokud databáze s názvem `ASPNETDB.mdf` byl přidán do `App_Data` složky, jedná o problém. Vrátit do kroku 4  *<a id="_msoanchor_10"> </a> [vytváření schématu členství v systému SQL Server](creating-the-membership-schema-in-sql-server-cs.md)*  kurz pokyny o tom, jak správně nakonfigurovat zprostředkovatele členství.


Ve většině vytvořit uživatele scénáře účtu, zobrazí návštěvníka některé rozhraní zadat svoje uživatelské jméno, heslo, e-mailu a další nezbytné informace v tomto okamžiku se vytvoří nový účet. V tomto kroku jsme zvážení vytvoření takového rozhraní ručně a pak viděli, jak používat `Membership.CreateUser` na vstupy uživatele na základě metod programově přidat nový uživatelský účet. Naše kód, ale právě vytvořili nový uživatelský účet. Není provedli všechny následné akce, například přihlášení uživatele k webu pomocí právě vytvořeného uživatelského účtu nebo odesílání e-mailu s potvrzením pro uživatele. Tyto další kroky by vyžadovaly další kód na tlačítku `Click` obslužné rutiny události.

ASP.NET je dodáván s ovládacím prvkem CreateUserWizard, který je určený pro zpracování procesu vytvoření účtu uživatele z vykreslování uživatelské rozhraní pro vytváření nových uživatelských účtů, k vytvoření účtu v rámci členství a provádění po účtu Vytvoření úloh, například odeslání e-mailu s potvrzením a přihlášením uživatele právě vytvořené na webu. Použití ovládacího prvku CreateUserWizard je jednoduché, přetahování ovládacího prvku CreateUserWizard z panelu nástrojů na stránku a pak nastavíte některé jeho vlastnosti. Ve většině případů nebude muset napsat jediný řádek kódu. Se podíváme na tento nifty ovládací prvek podrobně v kroku 6.

Pokud nové uživatelské účty jsou pouze vytvořen pomocí běžné webové stránky vytvořit účet, nepravděpodobné, že budete někdy muset psát kód, který používá `CreateUser` metoda, jako ovládací prvek CreateUserWizard bude pravděpodobně vyhovovat vašim potřebám. Ale `CreateUser` metoda je užitečný ve scénářích, kde je nutné vysoce přizpůsobenou činnost koncového uživatele vytvořit účet nebo když potřebujete prostřednictvím kódu programu vytvoříte nové uživatelské účty, alternativní rozhraní. Například můžete mít na stránce, která umožňuje uživatelům nahrát soubor XML, který obsahuje informace o uživateli z nějaké jiné aplikaci. Stránky může analyzuje obsah nahrávaný soubor XML souboru a vytvořit nový účet pro každého uživatele reprezentován v souboru XML voláním `CreateUser` metoda.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Krok 6: Vytvoření nového uživatele pomocí ovládacího prvku CreateUserWizard

ASP.NET se dodává s řadou ovládací prvky webového přihlášení. Tyto ovládací prvky podpory v mnoha běžné uživatelské souvisejícím s účtem a přihlášení scénáře. [Prvku CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) jeden takový ovládací prvek, který je určen k dispozici uživatelské rozhraní pro přidání nového uživatelského účtu do rozhraní členství.

Podobně jako mnoho jiných související s přihlášením webových ovládacích prvků CreateUserWizard lze bez nutnosti napsat jediný řádek kódu. Intuitivně poskytuje uživatelské rozhraní založené na zprostředkovateli členství nastavení konfigurace a interně volá `Membership` třídy `CreateUser` metoda poté, co uživatel zadá potřebné informace a klikne na tlačítko "Vytvořit uživatele". Ovládací prvek CreateUserWizard je velmi přizpůsobitelná. Existují hostitele událostí, které budou spuštěny při různých fází procesu vytváření účtu. Obslužné rutiny událostí, můžeme vytvořit podle potřeby vložení vlastní logiky do pracovní postup vytvoření účtu. Kromě toho CreateUserWizard vzhled je velmi flexibilní. Existuje několik vlastností, které definují vzhled výchozí rozhraní; v případě potřeby ovládacího prvku lze převést na šablonu, nebo lze přidat další uživatele registrace "kroky".

Začněme podívejte se na pomocí ovládacího prvku CreateUserWizard výchozí rozhraní a chování. Potom jsme budete prozkoumejte postup přizpůsobení vzhledu prostřednictvím vlastnosti a události ovládacího prvku.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Zkoumání CreateUserWizard výchozí rozhraní a chování

Vraťte se do `CreatingUserAccounts.aspx` stránku `Membership` složky, přepněte do režimu návrhu nebo rozdělení a poté přidejte ovládacího prvku CreateUserWizard do horní části stránky. Ovládací prvek CreateUserWizard je naplněn části přihlášení – ovládací prvky panelu nástrojů. Po přidání ovládacího prvku, nastavte jeho `ID` vlastnost `RegisterUser`. Jak v obrázku 11 ukazuje snímek obrazovky, vykreslí CreateUserWizard rozhraní s textových polí pro nového uživatele uživatelské jméno, heslo, e-mailovou adresu a bezpečnostní otázku a odpověď.


[![Ovládací prvek CreateUserWizard vykreslí obecný vytváření uživatelského rozhraní](creating-user-accounts-cs/_static/image32.png)](creating-user-accounts-cs/_static/image31.png)

**Obrázek 11**: ovládacího prvku CreateUserWizard vykreslí obecné vytvořit uživatelské rozhraní ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image33.png))


Podívejme se na chvíli k porovnání výchozí uživatelské rozhraní generované řízení CreateUserWizard pomocí rozhraní, které jsme vytvořili v kroku 5. Pro začátek ovládacího prvku CreateUserWizard umožňuje návštěvníka a zadat bezpečnostní otázku a odpověď, zatímco naše ručně vytvořit rozhraní používá předdefinovaného bezpečnostní otázku. Rozhraní CreateUserWizard ovládacího prvku také obsahuje ovládací prvky pro ověřování, zatímco jsme ještě měli implementovat ověření v našem rozhraní pole formuláře. A rozhraní ovládacího prvku CreateUserWizard obsahuje textové pole "Potvrďte heslo" (spolu s CompareValidator k Ujistěte se, zda text "Password" a "Porovnat heslo" textová pole jsou si rovny).

Co je zajímavé je, že ovládacího prvku CreateUserWizard zajímají nastavení konfigurace zprostředkovatele členství, při vykreslování svoje uživatelské rozhraní. Například bezpečnostní otázku a odpověď textových polí se zobrazí jenom v případě `requiresQuestionAndAnswer` nastaven na hodnotu True. Podobně CreateUserWizard automaticky přidá ovládací prvek RegularExpressionValidator zajistit, že jsou splněné požadavky na heslo síly a nastaví její `ErrorMessage` a `ValidationExpression` na základě vlastnosti `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`a `passwordStrengthRegularExpression` nastavení konfigurace.

Ovládací prvek CreateUserWizard, jak již název napovídá, je odvozený od [řízení průvodce](https://msdn.microsoft.com/library/s2etd1ek.aspx). Průvodce ovládací prvky jsou určeny k poskytnutí rozhraní pro provádění úloh, několika kroky. Řízení Průvodce může mít libovolný počet `WizardSteps`, z nichž každý je šablonu, která definuje HTML a webové ovládací prvky pro tento krok. Ovládací prvek Průvodce jeho otevření zobrazí první `WizardStep`, společně s ovládací prvky pro navigaci, aby umožňovaly uživatele z jednoho kroku pokračovat na další, nebo pro návrat k předchozí kroky.

Jak ukazuje deklarativní v obrázek 11, prvku CreateUserWizard výchozí rozhraní zahrnuje dvě`WizardSteps:`

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx)– vykreslí rozhraní ke shromažďování informací pro vytvoření nového uživatelského účtu. Toto je krok znázorňuje obrázek 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx)– vykreslí zprávu s upozorněním, že účet byla úspěšně vytvořena.

CreateUserWizard vzhled a chování lze změnit buď z těchto kroků převedením šablon, nebo můžete přidat vlastní `WizardSteps`. Podíváme se na přidání `WizardStep` rozhraní registrace v *ukládání Další informace o uživateli* kurzu.

Podíváme se na ovládací prvek CreateUserWizard v akci. Přejděte `CreatingUserAccounts.aspx` stránku prostřednictvím prohlížeče. Spustit v rámci některé neplatné hodnoty CreateUserWizard rozhraní. Zkuste zadat heslo, které neodpovídají požadavků na sílu hesel nebo výstupu "uživatele Name" textové pole prázdné. CreateUserWizard se zobrazí příslušná chybová zpráva. Obrázek 12 znázorňuje výstup při pokusu o vytvoření uživatele s nedostatečně silné heslo.


[![CreateUserWizard automaticky vloží ovládací prvky pro ověřování](creating-user-accounts-cs/_static/image35.png)](creating-user-accounts-cs/_static/image34.png)

**Obrázek 12**: CreateUserWizard automaticky vloží ověření ovládacích prvků ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image36.png))


Potom zadejte příslušné hodnoty do CreateUserWizard a klikněte na tlačítko "Vytvořit uživatele". Za předpokladu, že nebyla zadána požadovaná pole a síly hesla jsou dostatečné, bude CreateUserWizard vytvořit nový uživatelský účet, přes rozhraní členství a následně se zobrazí `CompleteWizardStep`je rozhraní (viz obrázek 13). Na pozadí CreateUserWizard volá `Membership.CreateUser` metoda, stejně jako jsme provedli v kroku 5.


[![Nový uživatelský účet má úspěšně vytvořena](creating-user-accounts-cs/_static/image38.png)](creating-user-accounts-cs/_static/image37.png)

**Obrázek 13**: A nový uživatelský účet je úspěšně vytvořená ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image39.png))


> [!NOTE]
> Jak ukazuje obrázek 13, `CompleteWizardStep`na rozhraní obsahuje tlačítko Pokračovat. V tomto okamžiku kliknutím právě však provede zpětné volání, ponechat návštěvníka na stejné stránce. V části "Přizpůsobení vzhledu a chování prostřednictvím jeho vlastnosti CreateUserWizard" se podíváme na tom, jak může mít toto tlačítko Odeslat musí návštěvníka `Default.aspx` (nebo některé jiné stránky).


Po vytvoření nového uživatelského účtu, vraťte se na Visual Studio a zkontrolujte `aspnet_Users` a `aspnet_Membership` tabulky, jako jsme provedli na obrázku 10, chcete-li ověřit, že účet byl úspěšně vytvořen.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Přizpůsobení chování a vzhledu prostřednictvím jeho vlastnosti CreateUserWizard

V mnoha různými způsoby, prostřednictvím vlastnosti, lze přizpůsobit CreateUserWizard `WizardSteps`a obslužných rutin událostí. V této části se podíváme na tom, jak přizpůsobit vzhled ovládacího prvku pomocí jeho vlastnosti; v další části zjistí rozšíření chování ovládacího prvku pomocí obslužných rutin událostí.

Prakticky všechny text zobrazovaný v prvku CreateUserWizard výchozí uživatelské rozhraní lze přizpůsobit prostřednictvím nadbytku jeho vlastnosti. Například "Uživatelské jméno", "Heslo", "Potvrdit heslo", "E-mailu", "Bezpečnostní otázku" a "Bezpečnostní otázce" štítky zobrazí nalevo od textová pole lze přizpůsobit pomocí [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), a [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)vlastnosti, v uvedeném pořadí. Stejně tak, jsou vlastnosti pro zadání text pro "Vytvořit uživatele" a "Pokračovat" tlačítka v `CreateUserWizardStep` a `CompleteWizardStep`, stejně, jako kdyby tato tlačítka se vykresluje jako tlačítka, LinkButtons nebo ImageButtons.

Barvy, ohraničení, písem a další vizuální prvky se dají konfigurovat přes hostitele vlastnosti stylu. Samotném ovládacím prvku CreateUserWizard má běžné webové ovládací prvek vlastnosti stylu – `BackColor`, `BorderStyle`, `CssClass`, `Font`a tak dále – a existuje několik vlastností stylu pro definování vzhled pro konkrétní části CreateUserWizard na rozhraní. [ `TextBoxStyle` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), například definuje styly pro textových polí v `CreateUserWizardStep`, při [ `TitleTextStyle` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definuje styl pro nadpis ("zaregistrovat pro vaše nové Účet").

Kromě vlastnosti týkající se vzhledu existuje celá řada vlastností, které ovlivňují chování CreateUserWizard ovládacího prvku. [ `DisplayCancelButton` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), pokud nastavena na hodnotu True, zobrazí tlačítko Zrušit vedle tlačítko "Vytvořit uživatele" (výchozí hodnota je False). Pokud chcete zobrazovat na tlačítko Storno, nezapomeňte také nastavit [ `CancelDestinationPageUrl` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), která určuje stránce uživatel posílá po kliknutí na tlačítko Storno. Jak jsme uvedli v předchozím oddílu, na tlačítko Pokračovat v `CompleteWizardStep`na rozhraní způsobí, že zpětné volání, ale ponechá návštěvníka na stejné stránce. Pokud chcete odeslat návštěvníka jinou stránku po kliknutí na tlačítko pokračovat, jednoduše zadejte adresu URL v [ `ContinueDestinationPageUrl` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Umožňuje aktualizovat `RegisterUser` prvku CreateUserWizard zobrazíte tlačítko Zrušit a k odeslání musí návštěvníka `Default.aspx` když jsou kliknutí na tlačítko Storno nebo pokračovat. K tomu, nastavte `DisplayCancelButton` vlastnost na hodnotu True a obě `CancelDestinationPageUrl` a `ContinueDestinationPageUrl` vlastnosti, které chcete "~ / Default.aspx". Obrázek 14 ukazuje aktualizované CreateUserWizard při zobrazení prostřednictvím prohlížeče.


[![Třída CreateUserWizardStep obsahuje tlačítko Zrušit](creating-user-accounts-cs/_static/image41.png)](creating-user-accounts-cs/_static/image40.png)

**Obrázek 14**: `CreateUserWizardStep` obsahuje tlačítko Zrušit ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image42.png))


Návštěvník zadá uživatelské jméno, heslo, e-mailovou adresu a bezpečnostní otázku a odpověď a klikne na tlačítko "Vytvořit uživatele", se vytvoří nový uživatelský účet a návštěvníka přihlášen jako že nově vytvořeného uživatele. Za předpokladu, že uživatel na stránce je vytvoření nového účtu sami, to je pravděpodobně toto chování žádoucí. Můžete však přidat nové uživatelské účty do skupiny správců. Při tom by být vytvořený uživatelský účet, ale správce zůstane přihlášený jako správce (a ne jako nově vytvořený účet). Toto chování může být upraveny prostřednictvím logickou hodnotu [ `LoginCreatedUser` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Uživatelské účty v rámci členství obsahují schválené příznak; Uživatelé, kteří nejsou schváleny se nemůže přihlásit do lokality. Ve výchozím nastavení je nově vytvořený účet označena jako schválení, které uživateli umožňují Přihlaste se k webu okamžitě. Je však možné, mají nové uživatelské účty, které jsou označeny jako neschválených. Případně můžete ručně schválení nových uživatelů, než můžete přihlášení; nebo možná chcete ověřit, zda je e-mailovou adresu zadali při registraci platný před umožňující k přihlášení uživatele. Ať může být tento případ, může mít nově vytvořený uživatelský účet, který je označen jako neschválených nastavením prvku CreateUserWizard [ `DisableCreatedUser` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) na hodnotu True (výchozí hodnota je False).

Zahrnout další související chování vlastnosti Poznámka `AutoGeneratePassword` a `MailDefinition`. Pokud [ `AutoGeneratePassword` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) nastaven na hodnotu True, `CreateUserWizardStep` nezobrazí textových polí "Password" a "Potvrďte heslo"; místo toho je heslo nově vytvořeného uživatele automaticky generovány pomocí `Membership` třídy [ `GeneratePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). `GeneratePassword` Metoda vytvoří heslo určené délky a dostatečný počet jiných než alfanumerických znaků pro uspokojení požadavků na sílu nastavené heslo.

[ `MailDefinition` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) je užitečné, pokud chcete odeslat e-mail na e-mailovou adresu zadanou během procesu vytváření účtů. `MailDefinition` Vlastnost obsahuje řadu podvlastnosti pro definování informace o vytvořený e-mailové zprávy. Tyto podvlastnosti zahrnovat možnosti jako `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, a `BodyFileName`. [ `BodyFileName` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) odkazuje na text nebo souboru HTML, který obsahuje tělo e-mailové zprávy. Text podporuje dva předdefinované symboly: `<%UserName%>` a `<%Password%>`. Tyto zástupné symboly, pokud je k dispozici v `BodyFileName` souboru, bude nahrazena adresou právě vytvořené uživatelské jméno a heslo.

> [!NOTE]
> `CreateUserWizard` Ovládacího prvku `MailDefinition` vlastnost právě Určuje podrobnosti o e-mailovou zprávu, která je odeslána při vytvoření nového účtu. Neobsahuje žádné informace o tom, jak je ve skutečnosti e-mailová zpráva (to znamená, zda je použit adresář serveru nebo vyřazení e-mailu SMTP, žádné informace o ověřování a podobně). Musí být definované v těchto nízké úrovně podrobnosti `<system.net>` kapitoly `Web.config`. Další informace o těchto nastaveních konfigurace a při odesílání e-mailu obecně z technologie ASP.NET 2.0, naleznete [nejčastější dotazy na SystemNetMail.com](http://www.systemnetmail.com/) a Moje článku [odesílání e-mailu v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Rozšíření CreateUserWizard chování pomocí obslužných rutin událostí

Ovládací prvek CreateUserWizard vyvolá určitý počet událostí během jejího pracovního postupu. Například po návštěvník zadá své uživatelské jméno, heslo a další údaje a klikne na tlačítko "Vytvořit uživatele", ovládacího prvku CreateUserWizard vyvolá jeho [ `CreatingUser` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Pokud dojde k problému při procesu vytvoření [ `CreateUserError` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) je aktivována například; ale, pokud byl uživatel úspěšně vytvořen, pak se [ `CreatedUser` událostí](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) se vyvolá. Existují další události ovládacího prvku CreateUserWizard, které získat vyvolány, ale toto jsou tři nejvíce podstatný ty.

V některých případech může chceme klepněte na do CreateUserWizard pracovního postupu, který jsme můžete provést vytvořením obslužné rutiny události pro příslušnou událost. Pro znázornění je můžeme vylepšit `RegisterUser` prvku CreateUserWizard zahrnout některé vlastního ověřování na uživatelské jméno a heslo. Konkrétně můžeme vylepšit naše CreateUserWizard tak, aby uživatelská jména nesmí obsahovat počáteční a koncové mezery a uživatelské jméno se nemůže vyskytovat kdekoli v hesle. Stručně řečeno chceme, aby někdo z vytváření jméno jako "Scott" nebo s kombinaci uživatelského jména a hesla jako "Scott" a "Scott.1234".

K tomu vytvoříme obslužné rutiny události pro `CreatingUser` událostí k provádění kontrol naše doplňující ověření. Pokud zadaná data nejsou platná, je potřeba zrušit procesu vytváření. Musíme přidání ovládacího prvku popisek na stránku a zobrazí zprávu, která vysvětluje, že uživatelské jméno nebo heslo je neplatné. Začněte tím, že přidání ovládacího prvku popisek pod ovládací prvek CreateUserWizard, nastavení jeho `ID` vlastnost `InvalidUserNameOrPasswordMessage` a jeho `ForeColor` vlastnost `Red`. Vymažte jeho `Text` vlastnost a sadu jeho `EnableViewState` a `Visible` vlastnosti na hodnotu False.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample7.aspx)]

Dále vytvořte obslužnou rutinu události pro ovládací prvek CreateUserWizard `CreatingUser` událostí. Pokud chcete vytvořit obslužnou rutinu události, vyberte ovládací prvek v návrháři a potom přejděte do okna vlastností. Zde klikněte na na ikonu bolt a potom dvakrát klikněte na příslušnou událost k vytvoření obslužné rutiny události.

Přidejte následující kód, který `CreatingUser` obslužné rutiny události:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample8.cs)]

Upozorňujeme, že jsou k dispozici prostřednictvím uživatelské jméno a heslo zadané do ovládacího prvku CreateUserWizard jeho [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) a [ `Password` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), v uvedeném pořadí. Ve výše uvedené obslužné rutiny události používáme tyto vlastnosti k určení, jestli se zadaným uživatelským jménem obsahuje počáteční a koncové mezery a zda je uživatelské jméno v rámci heslo nalezen. Pokud je splněná některá z těchto podmínek, chybová zpráva se zobrazí v `InvalidUserNameOrPasswordMessage` popisek a obslužné rutiny události `e.Cancel` je nastavena na `true`. Pokud `e.Cancel` je nastaven na `true`, CreateUserWizard zkratům jejího pracovního postupu, efektivně zrušení procesu vytvoření účtu uživatele.

Snímek obrazovky ukazuje obrázek 15 `CreatingUserAccounts.aspx` když uživatel zadá uživatelské jméno s úvodní mezery.


[![Uživatelská jména s úvodní a koncové mezery nejsou povolené.](creating-user-accounts-cs/_static/image44.png)](creating-user-accounts-cs/_static/image43.png)

**Obrázek 15**: uživatelských jmen s úvodní a koncové mezery nejsou povolené ([Kliknutím zobrazit obrázek v plné velikosti](creating-user-accounts-cs/_static/image45.png))


> [!NOTE]
> Vidíte příklad použití prvku CreateUserWizard `CreatedUser` událost v  *<a id="_msoanchor_11"> </a> [ukládání Další informace o uživateli](storing-additional-user-information-cs.md)*  kurzu.


## <a name="summary"></a>Souhrn

`Membership` Třídy `CreateUser` metoda vytvoří nový uživatelský účet v rámci členství. Dělá to tak delegováním volání nakonfigurovaného zprostředkovatele členství. U `SqlMembershipProvider`, `CreateUser` metoda přidá záznam o `aspnet_Users` a `aspnet_Membership` databázových tabulek.

Sice prostřednictvím kódu programu (jak jsme viděli v kroku 5), mohou být vytvořeny nové uživatelské účty, je rychlejší a snadnější přístup, použijte ovládací prvek CreateUserWizard. Tento ovládací prvek vykreslí vícekrokový uživatelské rozhraní pro shromažďování informací o uživatele a vytvoření nového uživatele v rámci členství. Pod zahrnuje, tento ovládací prvek používá stejnou `Membership.CreateUser` metoda jako zkontrolován v kroku 5, ale ovládací prvek vytvoří uživatelské rozhraní, ovládací prvky pro ověřování a reagují na chyby vytvoření účtu uživatele bez nutnosti psaní kódu na ikonu.

V tomto okamžiku máme funkce zavedené vytvoříte nové uživatelské účty. Však přihlašovací stránku stále je ověření proti těchto pevně přihlašovacích údajů, které jsme uvedli zpět v druhé kurzu. V <a id="_msoanchor_12"> </a> [další kurz](validating-user-credentials-against-the-membership-user-store-cs.md) aktualizujeme `Login.aspx` k ověření uživatele je zadaná pověření proti rozhraní členství.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [`CreateUser`Technická dokumentace](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Přehled ovládacího prvku CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Vytvoření zprostředkovatele mapy souboru založené na systému lokality](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Vytváření pomocí ovládacího prvku ASP.NET 2.0 průvodce krok za krokem uživatelské rozhraní](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Zkoumání ASP.NET 2.0 je navigace na webu](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Hlavní stránky a navigace na webu](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Zprostředkovatel SQL lokality mapy jste čekali pro](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování...

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Předchozí](creating-the-membership-schema-in-sql-server-cs.md)
[další](validating-user-credentials-against-the-membership-user-store-cs.md)
