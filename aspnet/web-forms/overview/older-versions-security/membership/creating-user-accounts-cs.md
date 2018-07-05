---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
title: Vytváření uživatelských účtů (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu se podíváme na používání rozhraní členství (prostřednictvím SqlMembershipProvider) k vytvoření nové uživatelské účty. Uvidíme, jak vytvořit nový nám...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: f175278c-6079-4d91-b9b4-2493ed43d9ec
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 161d7b71f169252699e0c33ad969bef9c2c62d5f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386595"
---
<a name="creating-user-accounts-c"></a>Vytváření uživatelských účtů (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_cs.pdf)

> V tomto kurzu se podíváme na používání rozhraní členství (prostřednictvím SqlMembershipProvider) k vytvoření nové uživatelské účty. Uvidíme, jak vytvořit nové uživatele prostřednictvím kódu programu a prostřednictvím ASP. NET pro integrované prvku CreateUserWizard.


## <a name="introduction"></a>Úvod

V <a id="_msoanchor_1"> </a> [předchozím kurzu](creating-the-membership-schema-in-sql-server-cs.md) schématu aplikací služby jsme nainstalovali v databázi, která přidá tabulek, zobrazení a uložené procedury, které jsou vyžadované `SqlMembershipProvider` a `SqlRoleProvider`. To vytvoří infrastrukturu, kterou potřebujeme pro zbytek této série kurzů. V tomto kurzu se podíváme na používání rozhraní členství (prostřednictvím `SqlMembershipProvider`) k vytvoření nové uživatelské účty. Uvidíme, jak vytvořit nové uživatele prostřednictvím kódu programu a prostřednictvím ASP. NET pro integrované prvku CreateUserWizard.

Kromě učit, jak vytvořit nové uživatelské účty, jsme také muset aktualizovat ukázkovému webu jsme v prvním vytvoření *<a id="_msoanchor_2"> </a> [Přehled ověřování založené na formulářích](../introduction/an-overview-of-forms-authentication-cs.md)* kurz a potom v rozšířené  *<a id="https://www.asp.net/learn/security/tutorial-03-cs.aspx"> </a> konfigurace ověřování formulářů a témata pokročilé* kurzu. Naše ukázkové webové aplikace má přihlašovací stránku, která ověřuje přihlašovacích údajů uživatelů, kteří páry pevně zakódované uživatelské jméno a heslo. Kromě toho `Global.asax` zahrnuje kód, který vytvoří vlastní `IPrincipal` a `IIdentity` objekty ověřených uživatelů. Budeme aktualizovat přihlašovací stránku k ověření přihlašovacích údajů uživatelů rámec členství a odeberte vlastní logiku zabezpečení a identity.

Pusťme se do práce!

## <a name="the-forms-authentication-and-membership-checklist"></a>Kontrolní seznam členství a ověřování pomocí formulářů

Předtím, než jsme začít pracovat s použitím rozhraní framework členství, věnujte chvíli důležité kroky, které jsme udělali k dosažení tohoto bodu. Pokud používáte rozhraní framework členství s `SqlMembershipProvider` v případě ověřování pomocí formulářů v následující kroky je nutné provést ještě před jejich implementací funkce členství ve webové aplikaci:

1. **Povolte ověřování pomocí formulářů.** Jak jsme probírali v  *<a id="_msoanchor_4"> </a> [Přehled ověřování založené na formulářích](../introduction/an-overview-of-forms-authentication-cs.md)*, ověřování pomocí formulářů je povoleno úpravou `Web.config` a nastavení `<authentication>` prvku `mode` atribut `Forms`. S povolené ověřování pomocí formulářů, je vyhledán jednotlivých příchozích požadavků *forms lístek ověřování*, který, pokud jsou k dispozici, identifikuje žadatel.
2. **Přidáte schéma aplikace služby k příslušné databázi.** Při použití `SqlMembershipProvider` potřebujeme k instalaci služby schématu aplikace k databázi. Toto schéma je obvykle přidány do stejné databáze, která obsahuje datový model aplikace. *<a id="_msoanchor_5"> </a> [Vytvoření schématu členství v SQL serveru](creating-the-membership-schema-in-sql-server-cs.md)* prohlédli jste si kurz pomocí `aspnet_regsql.exe` nástroj k provedení této.
3. **Nastavení webové aplikace tak, aby odkazovaly databázi z kroku 2.** *Vytvoření schématu členství v SQL serveru* kurz vám ukázal, dva způsoby, jak nakonfigurovat webovou aplikaci tak, aby `SqlMembershipProvider` byste použili databáze vybrali v kroku 2: úpravou `LocalSqlServer` název připojovacího řetězce; nebo přidáním nového registrovaného zprostředkovatele do seznamu zprostředkovatelů členství rozhraní framework a přizpůsobení tohoto nového poskytovatele. použijte databázi z kroku 2.

Při vytváření webové aplikace, které používá `SqlMembershipProvider` a ověřování pomocí formulářů, budete muset provést tyto tři kroky před použitím `Membership` třídy nebo ovládací prvky ASP.NET – webové přihlášení. Protože jsme už provedli tyto kroky v předchozích kurzech, budeme připravení začít používat rozhraní členství!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Přidání nové stránky ASP.NET

V tomto kurzu a další tři jsme se zkoumání různých funkcí souvisejících s členství a možnosti. Potřebujeme sérii stránek ASP.NET k implementaci témata prozkoumat v rámci těchto kurzů. Pojďme vytvořit tyto stránky a pak soubor mapy webu `(Web.sitemap)`.

Začněte tím, že vytvoříte novou složku v projektu s názvem `Membership`. V dalším kroku přidejte pět nových stránek technologie ASP.NET na `Membership` složky propojení s každou stránku `Site.master` stránky předlohy. Název stránky:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Průzkumník řešení vašeho projektu v tomto okamžiku by měl vypadat podobně jako obrazovky je vidět na obrázku 1.


[![Pět nových stránek byly přidány do složky členství](creating-user-accounts-cs/_static/image2.png)](creating-user-accounts-cs/_static/image1.png)

**Obrázek 1**: pět nových stránek byly přidány do `Membership` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image3.png))


Jednotlivé stránky, v tomto okamžiku má dva ovládací prvky obsahu, jeden pro každou z prvků ContentPlaceHolder na hlavní stránce: `MainContent` a `LoginContent`.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample1.aspx)]

Vzpomeňte si, že `LoginContent` ContentPlaceHolder jeho výchozí značky zobrazí odkaz na přihlášení nebo odhlášení uživatele na webu, v závislosti na tom, jestli je uživatel ověřený. Přítomnost `Content2` ovládací prvek obsahu, ale přepíše výchozí značky stránky předlohy. Jak jsme probírali v *<a id="_msoanchor_6"> </a> [Přehled ověřování založené na formulářích](../introduction/an-overview-of-forms-authentication-cs.md)* kurz, to je užitečné v stránky, kde jsme nechcete zobrazit související s přihlášením možnosti v levém sloupci.

U těchto pět stránek, ale má být zobrazena na hlavní stránce výchozí kód pro `LoginContent` ContentPlaceHolder. Proto odebrat deklarativní `Content2` ovládacího prvku obsahu. Až to uděláte, každý z pěti stránku značek by měl obsahovat pouze jeden ovládací prvek obsahu.

## <a name="step-2-creating-the-site-map"></a>Krok 2: Vytvoření mapy webu

Vše kromě nejvíce triviální weby muset implementovat nějakou formu navigační uživatelské rozhraní. Navigační uživatelské rozhraní může být jednoduchý seznam odkazů na různé části webu. Tyto odkazy můžete také může být uspořádány do nabídky nebo stromová zobrazení. Jako vývojáři stránek vytvoření navigační uživatelské rozhraní je pouze polovinu scénáře. Potřebujeme také některé znamená definovat strukturu logické lokality udržitelného a aktualizovatelné způsobem. Nové stránky se přidání nebo odebrání existujících stránek, chceme, abyste mohli aktualizovat jednozdrojového – Mapa webu – a mají tyto změny projeví v navigační uživatelské rozhraní v lokalitě.

Tyto dvě úlohy – definování mapy webu a implementaci navigační uživatelského rozhraní na základě mapy webu – jsou snadné díky rozhraní Mapa webu a ovládací prvky navigace Web přidat v technologii ASP.NET verze 2.0. Rozhraní Mapa webu umožňuje vývojářům definovat Mapa webu a pak k němu přistupovat prostřednictvím programových rozhraní API ( [ `SiteMap` třídy](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Zahrnují integrované webové navigační ovládací prvky [Menu – ovládací prvek](https://msdn.microsoft.com/library/bz09dy46.aspx), [ovládací prvek TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)a [ovládací prvek SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Jako jsou členství a rolí rozhraní framework mapy webu propojitelnosti [modelu poskytovatele](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Úlohy třídy zprostředkovatele mapy webu je k vytvoření struktury v paměti používané `SiteMap` třídy z do trvalého úložiště dat, jako je například soubor XML nebo databázové tabulce. Rozhraní .NET Framework se dodává s výchozí zprostředkovatele mapy webu, který čte data mapy webu ze souboru XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), a to je poskytovatel použijeme v tomto kurzu. Některé alternativní implementace zprostředkovatele mapy webu naleznete v části Další čtení na konci tohoto kurzu.

Výchozího zprostředkovatele mapy webu očekává, že správně ve formátu XML soubor s názvem `Web.sitemap` existovat kořenový adresář. Protože používáme tento výchozího zprostředkovatele, musíme přidat tento soubor a definovat strukturu mapy webu ve správném formátu XML. Chcete-li přidat soubor, klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a zvolte Přidat novou položku. V dialogovém okně vyjádřit souhlas se službou přidat soubor mapy webu s názvem typu `Web.sitemap`.


[![Přidejte soubor s názvem Web.sitemap do kořenového adresáře projektu](creating-user-accounts-cs/_static/image5.png)](creating-user-accounts-cs/_static/image4.png)

**Obrázek 2**: Přidat soubor s názvem `Web.sitemap` do kořenového adresáře projektu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image6.png))


Soubor mapy webu XML definuje strukturu webu jako hierarchie. Tento hierarchický vztah je modelovaná v souboru XML pomocí původ z `<siteMapNode>` elementy. `Web.sitemap` Musí začínat `<siteMap>` nadřazeného uzlu, který obsahuje přesně jednu `<siteMapNode>` podřízené. Tomto nejvyšší úrovně `<siteMapNode>` element reprezentuje kořen hierarchie a může mít libovolný počet podřízených uzlů. Každý `<siteMapNode>` musí obsahovat element `title` atribut a může volitelně zahrnovat `url` a `description` atributy, mimo jiné; každý neprázdný `url` atribut musí být jedinečný.

Zadejte následující kód XML do `Web.sitemap` souboru:

[!code-xml[Main](creating-user-accounts-cs/samples/sample2.xml)]

Výše uvedené značky mapy webu definuje hierarchii je znázorněno na obrázku 3.


[![Mapa webu představuje hierarchickou strukturu navigační](creating-user-accounts-cs/_static/image8.png)](creating-user-accounts-cs/_static/image7.png)

**Obrázek 3**: mapy webu představuje hierarchickou strukturu navigační ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Krok 3: Aktualizace stránky předlohy, aby zahrnovala navigační uživatelské rozhraní

Technologie ASP.NET obsahuje řadu související navigačního webové ovládací prvky k návrhu uživatelského rozhraní. Patří mezi ně v nabídce, prvek TreeView a ovládací prvky SiteMapPath. Ovládací prvky nabídky a TreeView struktury mapy webu v nabídky nebo stromu, vykreslit, že ovládací prvky SiteMapPath zobrazí s popisem cesty, který zobrazuje aktuální uzel přístupu a jeho nadřazenými prvky. Data mapy webu může být vázaný na jiná data webové ovládací prvky pomocí SiteMapDataSource a je možné programově přistupovat přes `SiteMap` třídy.

Důkladné diskuzi o rozhraní Mapa webu a ovládací prvky navigace je nad rámec této sérii kurzů, spíše než ztrácet čas vytváření vlastní navigační uživatelské rozhraní teď místo toho si půjčte použitému v mé *[ Práce s daty v technologii ASP.NET 2.0](../../data-access/index.md)* série kurzů, které používá ovládacím prvkem Repeater k zobrazení seznamu s odrážkami hloubkové dvou navigačních odkazů, jak je znázorněno na obrázku 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Přidání Dvouúrovňová seznam odkazů v levém sloupci

Pokud chcete vytvořit toto rozhraní, přidejte následující deklarativní k `Site.master` hlavní stránky v levém sloupci kde text "TODO: nabídky tady bude..." momentálně nachází.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample3.aspx)]

Výše uvedené značka vytvoří vazbu ovládacího prvku opakovače s názvem `menu` SiteMapDataSource, který vrací definované v hierarchii mapy webu `Web.sitemap`. Od verze ovládacího prvku SiteMapDataSource [ `ShowStartingNode` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) nastaven na hodnotu NEPRAVDA, spustí vrácení mapy webu hierarchii, počínaje následovníky uzel "Home". Opakovači zobrazuje každou z těchto uzlů (aktuálně jenom "členství") v `<li>` elementu. Jiný, vnitřní Repeater pak zobrazí podřízené objekty aktuálního uzlu ve vnořených neuspořádaný seznam.

Obrázek 4 ukazuje výše uvedené značky vykresleného výstupu se strukturou mapy webu, který jsme vytvořili v kroku 2. Opakovači vykreslí značky vanilla neuspořádaný seznam pravidla šablony kaskádových definované v `Styles.css` zodpovídají za vkusnou rozložení. Podrobnější popis toho, jak funguje výše uvedené značky, najdete [stránky předlohy a navigace na webu](https://asp.net/learn/data-access/tutorial-03-cs.aspx) kurzu.


[![Navigační uživatelské rozhraní je vykreslen pomocí vnořené neuspořádaný seznam](creating-user-accounts-cs/_static/image11.png)](creating-user-accounts-cs/_static/image10.png)

**Obrázek 4**: The navigační uživatelské rozhraní je vykreslen pomocí vnořené neuspořádaný seznam ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Přidání navigace s popisem cesty

Kromě seznam odkazů v levém sloupci, můžeme mít také každý zobrazení stránky [navigace s popisem cesty](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Zobrazení cesty je prvek navigační uživatelského rozhraní, které rychle uživatelům své aktuální pozici v rámci hierarchie lokality. Ovládací prvek SiteMapPath framework mapy webu používá k určení aktuální stránky umístění v mapě webu a pak zobrazí s popisem cesty, na základě těchto informací.

Konkrétně, přidat `<span>` element na stránce předlohy záhlaví `<div>` prvek a nastavte nový `<span>` elementu `class` atribut "s popisem cesty". ( `Styles.css` Třída obsahuje pravidla pro třídu "s popisem cesty".) V dalším kroku přidejte SiteMapPath na tomto nový `<span>` elementu.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample4.aspx)]

Obrázek 5 ukazuje výstup ovládací prvky SiteMapPath při návštěvě `~/Membership/CreatingUserAccounts.aspx`.


[![Zobrazí tento navigační prvek určuje aktuální stránku a namapujte jeho předchůdců v lokalitě](creating-user-accounts-cs/_static/image14.png)](creating-user-accounts-cs/_static/image13.png)

**Obrázek 5**: Tento navigační prvek určuje zobrazí aktuální stránce a jeho nadřazenými prvky do mapy webu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Krok 4: Odebrání vlastního objektu zabezpečení a Identity logiku

V *<a id="_msoanchor_7"> </a> [konfigurace ověřování formulářů a témata pokročilé](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)* kurzu jsme viděli, jak přidružit vlastní objekty zabezpečení a identity ověřeného uživatele. Můžeme to provést tak, že vytvoříte obslužné rutiny události v `Global.asax` pro aplikace `PostAuthenticateRequest` událost, která se vyvolá po `FormsAuthenticationModule` ověření uživatele. V této obslužné rutiny události jsme nahradit `GenericPrincipal` a `FormsIdentity` objekty přidá `FormsAuthenticationModule` s `CustomPrincipal` a `CustomIdentity` objekty jsme vytvořili v tomto kurzu.

Vlastní objekty zabezpečení a identita jsou užitečné v některých scénářích, ve většině případů `GenericPrincipal` a `FormsIdentity` objekty jsou dostačující. V důsledku toho myslím, že bylo by vhodné si vrátit k výchozímu chování. Odebráním nebo okomentováním odpovídajícího provedení této změny `PostAuthenticateRequest` obslužná rutina události nebo odstraněním `Global.asax` kompletně soubor.

## <a name="step-5-programmatically-creating-a-new-user"></a>Krok 5: Programové vytvoření nového uživatele

Chcete-li vytvořit nový uživatelský účet pomocí rozhraní framework členství `Membership` třídy [ `CreateUser` metoda](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Tato metoda má vstupní parametry pro uživatelské jméno, heslo a další související s uživateli pole. Na volání, deleguje vytvoření nového uživatelského účtu nakonfigurovaného zprostředkovatele členství a potom se vrátí [ `MembershipUser` objekt](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) představující nově vytvořené uživatelský účet.

`CreateUser` Metoda má čtyři přetížení, každé přijímající různý počet vstupních parametrů:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Tyto čtyři přetížení se liší v množství informací, které se shromažďují. První přetížení, například vyžaduje jenom uživatelské jméno a heslo pro nový uživatelský účet, zatímco druhá také vyžaduje e-mailovou adresu uživatele.

Tato přetížení neexistuje, protože informace potřebné k vytvoření nového uživatelského účtu závisí na nastavení konfigurace zprostředkovatele členství. V *<a id="_msoanchor_8"> </a> [vytvoření schématu členství v SQL serveru](creating-the-membership-schema-in-sql-server-cs.md)* kurzu jsme se zaměřili na zadání nastavení konfigurace pro zprostředkovatele členství v `Web.config`. Tabulka 2 zahrnuté úplný seznam nastavení konfigurace.

Jeden takový členství zprostředkovatele nastavení konfigurace, který má vliv na co `CreateUser` lze přetížení je `requiresQuestionAndAnswer` nastavení. Pokud `requiresQuestionAndAnswer` je nastavena na `true` (výchozí), pak při vytváření nového uživatelského účtu jsme musíte zadat bezpečnostní otázky a odpovědi. Tyto informace se později používá, pokud uživatel musí resetovat nebo změnit své heslo. Konkrétně v tu chvíli jsou zobrazená na bezpečnostní otázku a musí uvést správnou odpověď, aby bylo možné resetovat nebo změnit své heslo. V důsledku toho pokud `requiresQuestionAndAnswer` je nastavena na `true` pak buď první dvě volání `CreateUser` přetížení má za následek výjimku, protože chybí zabezpečovací otázka a odpověď. Vzhledem k tomu, že naše aplikace je nakonfigurován tak, aby vyžadovala zabezpečovací otázka a odpověď, budeme muset použít jeden z druhé dvě přetížení při vytváření uživatele prostřednictvím kódu programu.

Pro ilustraci použití `CreateUser` metoda, Pojďme vytvořit uživatelské rozhraní, ve kterém jsme výzva k zadání jména, hesla, e-mailu a odpověď na předdefinované bezpečnostní otázky. Otevřít `CreatingUserAccounts.aspx` stránku `Membership` složky a přidejte následující ovládací prvky webového obsahu ovládacího prvku:

- Textové pole s názvem `Username`
- Textové pole s názvem `Password`, jejichž `TextMode` je nastavena na `Password`
- Textové pole s názvem `Email`
- Popisek s názvem `SecurityQuestion` s jeho `Text` vlastnost odstraněné
- Textové pole s názvem `SecurityAnswer`
- Tlačítko s názvem `CreateAccountButton` jejichž Text je nastavena na "Vytvořit uživatelský účet"
- Ovládací prvek popisek s názvem `CreateAccountResults` s jeho `Text` vlastnost odstraněné

V tomto okamžiku vaše obrazovka by měla vypadat podobně jako obrazovky je vidět na obrázku 6.


[![Přidat různé ovládací prvky webové stránky CreatingUserAccounts.aspx](creating-user-accounts-cs/_static/image17.png)](creating-user-accounts-cs/_static/image16.png)

**Obrázek 6**: přidat různé webové ovládací prvky `CreatingUserAccounts.aspx` stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image18.png))


`SecurityQuestion` Popisek a `SecurityAnswer` textové pole se mají zobrazit předdefinované bezpečnostní otázky a odpovědi uživatele shromažďování. Všimněte si, zabezpečovací otázka a odpověď jsou uložená na základě uživatele podle uživatelského tak je možné, aby každý uživatel mohl definovat vlastní bezpečnostní otázku. Ale pro účely tohoto příkladu rozhodli používat univerzální bezpečnostní otázky, konkrétně: "Co je Oblíbené barva?"

K implementaci této předdefinované bezpečnostní otázku, přidejte konstanty do třídy použití modelu code-behind na stránce s názvem `passwordQuestion`, její přiřazení na bezpečnostní otázku. Potom v `Page_Load` obslužná rutina události, přiřaďte tuto konstanta, která se `SecurityQuestion` popisku `Text` vlastnost:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample5.cs)]

Dále vytvořte obslužnou rutinu události pro `CreateAccountButton`společnosti `Click` událostí a přidejte následující kód:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample6.cs)]

`Click` Spustí obslužnou rutinu události tak, že definujete proměnnou s názvem `createStatus` typu [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` je výčet, který označuje stav `CreateUser` operace. Například, pokud uživatelský účet je vytvořen úspěšně, výsledná `MembershipCreateStatus` instance bude nastavena na hodnotu `Success`; na druhé straně, pokud se operace nezdaří, protože už existuje uživatel se stejným uživatelským jménem, bude nastavena na hodnotu `DuplicateUserName`. V `CreateUser` přetížení používáme, potřebujeme k předání `MembershipCreateStatus` instance do metody jako `out` parametru. Tento parametr je nastaven na odpovídající hodnotu v rámci `CreateUser` metoda a můžete zkontrolovat její hodnota po volání metody k určení, zda uživatelský účet byl úspěšně vytvořen.

Po volání `CreateUser`a předejte `createStatus`, `switch` prohlášení se používá k výstupu odpovídající zprávu v závislosti na hodnotě přiřazené `createStatus`. Obrázky 7 zobrazuje výstup, když nového uživatele byla úspěšně vytvořena. Obrázky 8 a 9 popisují výstup, pokud nevytvoříte uživatelský účet. Na obrázku 8 návštěvníka zadali heslo pět nedoručených zpráv, který nesplňuje požadavky na sílu hesla států v nastavení konfigurace zprostředkovatele členství. Na obrázku 9 se pokouší návštěvníka vytvořte účet uživatele s existující uživatelské jméno (tu vytvořili na obrázku 7).


[![Nový uživatelský účet je úspěšně vytvořen](creating-user-accounts-cs/_static/image20.png)](creating-user-accounts-cs/_static/image19.png)

**Obrázek 7**: A nový uživatelský účet se úspěšně vytvořila ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image21.png))


[![Uživatelský účet nebyl vytvořen, protože zadané heslo je moc slabé](creating-user-accounts-cs/_static/image23.png)](creating-user-accounts-cs/_static/image22.png)

**Obrázek 8**: uživatelský účet nebyl vytvořen, protože zadané heslo je moc slabé ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image24.png))


[![Uživatelský účet je, že není vytvořena protože uživatelské jméno je již používáno](creating-user-accounts-cs/_static/image26.png)](creating-user-accounts-cs/_static/image25.png)

**Obrázek 9**: uživatelský účet není vytvořena protože uživatelské jméno je již používán ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image27.png))


> [!NOTE]
> Asi vás zajímá jak určit úspěch nebo neúspěch při jedním z prvních dvou `CreateUser` přetížení metody, ani o které má parametr typu `MembershipCreateStatus`. Vyvolat tyto první dvě přetížení [ `MembershipCreateUserException` výjimka](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) i v případě selhání, což zahrnuje [ `StatusCode` vlastnost](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) typu `MembershipCreateStatus`.


Po vytvoření jen několik uživatelských účtů, ověřte, že účty byly vytvořeny ve výpisu obsahu `aspnet_Users` a `aspnet_Membership` tabulky v `SecurityTutorials.mdf` databáze. Jak je vidět na obrázku 10, po přidání dva uživatele prostřednictvím `CreatingUserAccounts.aspx` stránky: Tito a Bruce.


[![Existují dva uživatele v Store uživatele členství: Tito a Bruce](creating-user-accounts-cs/_static/image29.png)](creating-user-accounts-cs/_static/image28.png)

**Obrázek 10**: existují dva uživatele v Store uživatele členství: Tito a Bruce ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image30.png))


Zatímco úložiště uživatele členství teď obsahuje informace o účtu Bruce a Tito společnosti, musíme ještě implementovat funkci, která umožňuje Bruce nebo Tito pro přihlášení k webu. V současné době `Login.aspx` ověřuje pověření uživatele s pevně zakódované sada párů uživatelského jména a hesla – dělá *není* ověření zadané přihlašovací údaje rámec členství. Pro nyní zobrazuje nové uživatelské účty v `aspnet_Users` a `aspnet_Membership` tabulky budou mít k stačit. V dalším kurzu  *<a id="_msoanchor_9"> </a> [ověření uživatele přihlašovací údaje proti the členství uživatele Store](validating-user-credentials-against-the-membership-user-store-cs.md)*, aktualizujeme na přihlašovací stránku k ověření proti úložišti členství.

> [!NOTE]
> Pokud nevidíte všechny uživatele ve vaší `SecurityTutorials.mdf` databáze, může to být způsobené vaše webová aplikace používá výchozí zprostředkovatel členství `AspNetSqlMembershipProvider`, který používá `ASPNETDB.mdf` databáze jako své úložiště uživatele. Pokud chcete zjistit, zda se jedná o problém, klikněte na tlačítko Aktualizovat v Průzkumníku řešení. Pokud databázi s názvem `ASPNETDB.mdf` byl přidán do `App_Data` složky, to je problém. Vraťte se do kroku 4 *<a id="_msoanchor_10"> </a> [vytvoření schématu členství v SQL serveru](creating-the-membership-schema-in-sql-server-cs.md)* kurz pokyny o tom, jak správně nakonfigurovat zprostředkovatele členství.


Ve většině vytvoření uživatelského účtu scénáře, návštěvníka prezentována některých rozhraní a zadejte své uživatelské jméno, heslo, e-mail a další důležité informace v tomto okamžiku se vytvoří nový účet. V tomto kroku jsme zvažovali vytváření takových rozhraní ručně a pak viděli, jak používat `Membership.CreateUser` na vstupy uživatele na základě metod programově přidat nový uživatelský účet. Náš kód, ale právě vytvořili nový uživatelský účet. Nebyla provedena jakékoli následné akce, jako jsou přihlášení uživatele k webu pomocí nově vytvořené uživatelského účtu nebo odeslání potvrzovací e-mail uživateli. Tyto další kroky by vyžadoval další kód na tlačítku `Click` obslužné rutiny události.

Technologie ASP.NET se dodává s ovládacím prvku CreateUserWizard, která je navržena pro zpracování procesu vytvoření účtu uživatele od vykreslení uživatelské rozhraní pro vytváření nových uživatelských účtů, k vytvoření účtu v rámci členství a provádění po účtu vytvoření úlohy, jako je odesílání e-mail s potvrzením a přihlašování nově vytvořené uživatelské Web. Použití ovládacího prvku CreateUserWizard je stejně jednoduché jako ovládacím prvku CreateUserWizard přetažením z panelu nástrojů na stránku a pak nastavit několik vlastností. Ve většině případů nemusíte napsat jediný řádek kódu. Budeme se věnovat podrobně v kroku 6 tohoto nifty ovládacího prvku.

Pokud nové uživatelské účty jsou vytvořeny pouze prostřednictvím běžné webové stránce vytvořit účet, je pravděpodobné, že někdy musíte napsat kód, který se používá `CreateUser` metody jako ovládacím prvku CreateUserWizard pravděpodobně vyhoví vašim potřebám. Ale `CreateUser` metoda je užitečné ve scénářích tam, kde potřebujete vysoce přizpůsobené uživatelské prostředí vytvořit účet, nebo když potřebujete k vytváření nových uživatelských účtů pomocí alternativních rozhraní prostřednictvím kódu programu. Například může mít stránku, která umožňuje uživatelům odeslat soubor XML, který obsahuje informace o uživateli z některé aplikace. Na stránce může analyzuje obsah v nahraném souboru XML souboru a vytvořit nový účet pro každého uživatele reprezentován v souboru XML pomocí volání `CreateUser` metody.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Krok 6: Vytvoření nového uživatele s ovládacím prvku CreateUserWizard

Technologie ASP.NET se dodává s celou řadou přihlášení webové ovládací prvky. Tyto ovládací prvky podpory v mnoha běžných uživatelů související s účtem a přihlášením scénářů. [Prvku CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) jeden takový ovládací prvek, který je navržený pro uživatelské rozhraní pro přidávání nového uživatelského účtu rozhraní Framework členství.

Stejně jako mnoho dalších ovládacích prvků webového související s přihlášením je možné CreateUserWizard aniž byste museli napsat jediný řádek kódu. Intuitivně poskytuje uživatelské rozhraní založené na zprostředkovateli členství nastavení konfigurace a interně volá `Membership` třídy `CreateUser` metoda poté, co uživatel zadá potřebné informace a klikne na tlačítko "Create User". Je mimořádně přizpůsobitelné ovládacím prvku CreateUserWizard. Existují celou řadu událostí, které se aktivují v různých fázích procesu vytvoření účtu. Obslužné rutiny událostí, můžeme vytvořit podle potřeby vložení vlastní logiky do pracovní postup vytvoření účtu. Kromě toho je velmi flexibilní vzhled prvku CreateUserWizard. Existuje mnoho vlastností, které definují vzhled rozhraní. výchozí hodnota v případě potřeby ovládací prvek lze převést na šablonu nebo lze přidat další uživatele registrace "takto".

Začněme podívat na výchozí rozhraní a chování ovládacího prvku CreateUserWizard. Potom podíváme, jak přizpůsobit vzhled přes vlastnosti a události ovládacího prvku.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Zkoumání výchozí rozhraní a chování CreateUserWizard.

Zpět `CreatingUserAccounts.aspx` stránku `Membership` složku, přepněte do režimu návrhu nebo rozdělit a pak přidejte do horní části stránky ovládacím prvku CreateUserWizard. Části ovládacích prvků panelu nástrojů přihlášení je zaznamenaná ovládacím prvku CreateUserWizard. Po přidání ovládacího prvku, nastavte jeho `ID` vlastnost `RegisterUser`. Jak v 11 obrázek ukazuje snímek obrazovky, CreateUserWizard vykreslí rozhraní s textová pole pro nového uživatele uživatelské jméno, heslo, e-mailovou adresu a bezpečnostní otázku a odpověď.


[![Vykreslení ovládacího prvku CreateUserWizard obecný vytváření uživatelského rozhraní](creating-user-accounts-cs/_static/image32.png)](creating-user-accounts-cs/_static/image31.png)

**Obrázek 11**: ovládacím prvku CreateUserWizard vykreslí obecný vytvořit uživatelské rozhraní ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image33.png))


Věnujte chvíli porovnat výchozí uživatelské rozhraní generovaných ovládacím prvku CreateUserWizard rozhraní, které jsme vytvořili v kroku 5. Pokud začínáte ovládacím prvku CreateUserWizard umožňuje návštěvníka zadat bezpečnostní otázku a odpověď, vzhledem k tomu použít naše rozhraní ručně vytvořené předdefinované bezpečnostní otázky. Rozhraní ovládacího prvku CreateUserWizard také validačních ovládacích prvků, že jsme museli ještě implementace ověření na naše rozhraní pole formuláře. A interface ovládacího prvku CreateUserWizard obsahuje textové pole "Potvrdit heslo" (spolu s CompareValidator zajistíte, že text zadaný "Password" a "Password porovnání" textová pole jsou si rovny).

Co je zajímavé je, že ovládacím prvku CreateUserWizard consults nastavení konfigurace zprostředkovatele členství, při generování uživatelského rozhraní. Například textová pole pro otázky a odpovědi zabezpečení se zobrazí jenom v případě `requiresQuestionAndAnswer` je nastavena na hodnotu True. Obdobně CreateUserWizard automaticky přidá ovládací prvek RegularExpressionValidator zajistíte, že jsou splněné požadavky na sílu hesla a nastaví její `ErrorMessage` a `ValidationExpression` na základě vlastnosti `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`a `passwordStrengthRegularExpression` nastavení konfigurace.

Ovládacím prvku CreateUserWizard jak již název napovídá, je odvozen z [ovládacího prvku průvodce](https://msdn.microsoft.com/library/s2etd1ek.aspx). Ovládací prvky průvodce jsou určená k poskytnutí rozhraní pro dokončení vícekrokových úkolů. Průvodce ovládací prvek může mít libovolný počet `WizardSteps`, z nichž každý je šablona definující kód HTML a webové ovládací prvky pro daný krok. Průvodce ovládací prvek zpočátku zobrazí první `WizardStep`, spolu s navigační ovládací prvky, které umožňují uživateli, pokračujte k dalšímu z jednoho kroku nebo se vrátit k předchozí kroky.

Jak ukazuje obrázek 11 deklarativní prvku CreateUserWizard výchozí rozhraní zahrnuje dva `WizardSteps:`

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) – vykreslí rozhraní ke shromažďování informací pro vytvoření nového uživatelského účtu. Tento krok je znázorněno na obrázku 11 je.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) – vykreslí zprávu s oznámením, že účet úspěšně vytvořil.

Vzhled a chování CreateUserWizard je možné upravit buď tyto kroky převedením na šablony, nebo tak, že přidáte vlastní `WizardSteps`. Se podíváme na přidání `WizardStep` rozhraní registrace v *ukládání Další informace o uživateli* kurzu.

Podívejme se na ovládacím prvku CreateUserWizard v akci. Přejděte `CreatingUserAccounts.aspx` stránky prostřednictvím prohlížeče. Začněte tím, že zadáte nějaké neplatné hodnoty CreateUserWizard rozhraní. Zkuste zadat heslo, které neodpovídají požadavků na sílu hesla nebo výstupu "uživatelské jméno" textové pole prázdné. CreateUserWizard se zobrazí příslušná chybová zpráva. Obrázek 12 se zobrazí výstup při pokusu o vytvoření uživatele s nedostatečně silné heslo.


[![CreateUserWizard automaticky vloží validačních ovládacích prvků](creating-user-accounts-cs/_static/image35.png)](creating-user-accounts-cs/_static/image34.png)

**Obrázek 12**: The CreateUserWizard automaticky vloží validačních ovládacích prvků ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image36.png))


Dále zadejte odpovídající hodnoty do CreateUserWizard a klikněte na tlačítko "Create User". Za předpokladu, že nebyla zadána požadovaná pole a síly hesla stačí, CreateUserWizard vytvořit nový uživatelský účet, prostřednictvím členství v rámci, který se pak zobrazí `CompleteWizardStep`v rozhraní (viz obrázek 13). Na pozadí CreateUserWizard volá `Membership.CreateUser` metody, stejně jako jsme to udělali v kroku 5.


[![Nový uživatelský účet byl úspěšně vytvořen](creating-user-accounts-cs/_static/image38.png)](creating-user-accounts-cs/_static/image37.png)

**Obrázek 13**: A nový uživatelský účet byl úspěšně vytvořen ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image39.png))


> [!NOTE]
> Jak ukazuje obrázek 13, `CompleteWizardStep`od rozhraní obsahuje tlačítko pro pokračování. Ale v tuto chvíli kliknete právě provádí zpětné volání návštěvníka opuštění na stejné stránce. V části "Přizpůsobení vzhledu a chování prostřednictvím její vlastnosti CreateUserWizard" se podíváme na tom, jak může mít toto tlačítko Odeslat musí návštěvníka `Default.aspx` (nebo některé jiné stránky).


Po vytvoření nového uživatelského účtu, vraťte se do sady Visual Studio a zkoumat `aspnet_Users` a `aspnet_Membership` tabulky, jak jsme to udělali na obrázku 10 ověření, že účet byl úspěšně vytvořen.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Přizpůsobení chování a vzhledu prostřednictvím její vlastnosti CreateUserWizard.

Řadu způsobů, prostřednictvím vlastnosti, je možné přizpůsobit CreateUserWizard `WizardSteps`a obslužné rutiny událostí. V této části se podíváme na tom, jak přizpůsobit vzhled ovládacího prvku prostřednictvím její vlastnosti; Další části se probírají rozšíření chování ovládacího prvku do obslužné rutiny událostí.

Téměř všechny textu zobrazeného v prvku CreateUserWizard výchozí uživatelské rozhraní je možné přizpůsobit pomocí jeho adresy vlastnosti. Například "Uživatelské jméno", "Password", "Potvrdit heslo", "E-mailu", "Bezpečnostní otázku" a "Zabezpečovací odpověď" popisků nalevo od textových polí můžete přizpůsobit [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), a [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)vlastnosti, v uvedeném pořadí. Podobně, jsou vlastnosti pro určení text pro tlačítko "Create User" a "Continue" v `CreateUserWizardStep` a `CompleteWizardStep`, stejně jako by tato tlačítka jsou vykresleny jako tlačítka, LinkButtons nebo ImageButtons.

Barvy, ohraničení, písem a další vizuální prvky se dají konfigurovat přes celou řadu vlastnosti stylu. Samotném ovládacím prvku CreateUserWizard má běžné webové ovládací prvek vlastnosti style – `BackColor`, `BorderStyle`, `CssClass`, `Font`, a tak dále – a existuje mnoho vlastností stylu pro definování vzhledu pro konkrétní části Rozhraní prvku CreateUserWizard. [ `TextBoxStyle` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), například definuje styly pro textových polí v `CreateUserWizardStep`, při [ `TitleTextStyle` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definuje styl pro nadpis ("zaregistrovat pro vaše nové Účet").

Kromě vlastnosti související s vzhled jsou k dispozici řada vlastností, které ovlivňují chování ovládacího prvku CreateUserWizard. [ `DisplayCancelButton` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), pokud nastavena na hodnotu True, zobrazí tlačítko Storno vedle tlačítko "Create User" (výchozí hodnota je False). Pokud chcete zobrazovat tlačítko Storno, nezapomeňte také nastavit [ `CancelDestinationPageUrl` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), která určuje stránky je uživatel přesměrován po kliknutí na tlačítko Storno. Jak je uvedeno v předchozí části, tlačítka pro pokračování v `CompleteWizardStep`od rozhraní vyvolá zpětné volání, ale ponechá návštěvníka na stejné stránce. K odeslání návštěvníka jinou stránku po kliknutí na tlačítko pokračovat, jednoduše zadejte adresu URL v [ `ContinueDestinationPageUrl` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Aktualizaci Pojďme `RegisterUser` prvku CreateUserWizard zobrazeno tlačítko Storno a posílat musí návštěvníka `Default.aspx` při klepnutí na tlačítko Zrušit nebo pokračovat. Chcete-li to provést, nastavte `DisplayCancelButton` vlastnost na hodnotu True a obě `CancelDestinationPageUrl` a `ContinueDestinationPageUrl` vlastnosti "~ / Default.aspx". Obrázek 14 zobrazí aktualizovaný CreateUserWizard při prohlížení prostřednictvím prohlížeče.


[![Třídu CreateUserWizardStep obsahuje tlačítko Storno](creating-user-accounts-cs/_static/image41.png)](creating-user-accounts-cs/_static/image40.png)

**Obrázek 14**: `CreateUserWizardStep` obsahuje tlačítko Storno ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image42.png))


Návštěvník zadá uživatelské jméno, heslo, e-mailovou adresu a bezpečnostní otázku a odpověď a klikne na tlačítko "Create User", je vytvořen nový uživatelský účet a je zaznamenána návštěvníka nově vytvořený uživateli. Za předpokladu, že uživatel na stránce je vytvoření nového účtu sami, to je pravděpodobně požadované chování. Můžete ale přidat nové uživatelské účty do skupiny správců. Přitom by být vytvořený uživatelský účet, ale správce zůstane přihlášený jako správce (a ne jako nově vytvořeného účtu). Toto chování lze změnit prostřednictvím logickou hodnotu [ `LoginCreatedUser` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Uživatelské účty v rámci členství obsahují schválené příznak; Uživatelé, kteří nejsou schválení se nemůže přihlásit na web. Ve výchozím nastavení nově vytvořeného účtu je označen jako schválenou, které uživateli umožňují okamžitě přihlásit webu. Je však možné, mají nové uživatelské účty, které jsou označené jako neschválené. Můžete chtít správce ručně schvalovat noví uživatelé předtím, než můžete protokolovat v; nebo možná budete chtít ověřte, zda je e-mailovou adresu zadali při registraci platná před umožňující přihlášení uživatele. Všechno, co může být případ, může mít nově vytvořený uživatelský účet, který je označené jako neschválené nastavením prvku CreateUserWizard [ `DisableCreatedUser` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) na hodnotu True (výchozí hodnota je False).

Zahrnout další vlastnosti poznámky týkající se chování `AutoGeneratePassword` a `MailDefinition`. Pokud [ `AutoGeneratePassword` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) nastavena na hodnotu True, `CreateUserWizardStep` nezobrazuje textová pole "Password" a "Potvrdit heslo"; místo toho je heslo nově vytvořeného uživatele automaticky generovány pomocí `Membership` třídy [ `GeneratePassword` metoda](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). `GeneratePassword` Metoda vytvoří heslo zadané délky a s dostatečný počet jiných než alfanumerických znaků pro splnění požadavků na sílu hesla nakonfigurované.

[ `MailDefinition` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) je užitečné, pokud chcete odesílat e-mailu a e-mailovou adresu zadali během procesu vytvoření účtu. `MailDefinition` Vlastnost obsahuje řadu objektu třídy subproperties pro definování informací o konstruovaný e-mailové zprávy. Tyto objektu třídy subproperties zahrnují možnosti jako `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, a `BodyFileName`. [ `BodyFileName` Vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) odkazuje na text nebo HTML soubor, který obsahuje tělo e-mailové zprávy. Tělo podporuje dvě předdefinované zástupné symboly: `<%UserName%>` a `<%Password%>`. Tyto zástupné symboly, pokud jsou k dispozici v `BodyFileName` souboru, nahradí se nově vytvořené uživatelské jméno a heslo.

> [!NOTE]
> `CreateUserWizard` Ovládacího prvku `MailDefinition` vlastnost určuje pouze podrobnosti o e-mailovou zprávu, která se odešle, když je vytvořen nový účet. Nezahrnuje všechny podrobnosti o jak skutečně přijde e-mailové zprávy (to znamená, určuje, zda se používá SMTP serveru nebo doručení pošty adresář, informace o ověřování a tak dále). Tyto podrobnosti nízké úrovně, musíte je definovat v `<system.net>` tématu `Web.config`. Další informace o těchto nastaveních konfigurace a odesílání e-mailu z technologie ASP.NET 2.0 obecně najdete [nejčastější dotazy na SystemNetMail.com](http://www.systemnetmail.com/) a Moje článku [odesílání e-mailu v ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Rozšíření chování CreateUserWizard pomocí obslužných rutin událostí

Počet událostí ovládacím prvku CreateUserWizard vyvolá během jejího pracovního postupu. Například po návštěvník zadá své uživatelské jméno, heslo a další důležité informace a klikne na tlačítko "Create User", ovládacím prvku CreateUserWizard vyvolá jeho [ `CreatingUser` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Pokud dojde k potížím během procesu vytvoření [ `CreateUserError` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) se aktivuje; ale, pokud je uživatel vytvořen úspěšně, pak bude [ `CreatedUser` události](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) je vyvolána. Existují další události ovládacího prvku CreateUserWizard, které získáte, ale toto jsou tři ty největší poskytnuty.

V některých scénářích možná chcete využijte CreateUserWizard pracovního postupu, které můžeme udělat tak, že vytvoříte obslužnou rutinu události pro příslušnou událost. Pro ilustraci to můžeme vylepšit `RegisterUser` prvku CreateUserWizard zahrnout některé vlastní ověřovací na uživatelské jméno a heslo. Konkrétně můžeme vylepšit naše CreateUserWizard tak, aby uživatelská jména nesmí obsahovat počáteční ani koncové mezery a uživatelské jméno se nemůže objevit kdekoli v hesle. Stručně řečeno chceme zabránit ve vytváření jméno jako je "Scott", nebo s kombinaci uživatelského jména a hesla jako "Scott" a "Scott.1234".

K tomu vytvoříme obslužná rutina události `CreatingUser` události k provádění naše další ověřovací kontroly. Pokud zadaná data nejsou platná potřebujeme zrušit v procesu vytváření. Musíme také přidat ovládací prvek popisek Web na stránku a zobrazí se zpráva s vysvětlením, že uživatelské jméno nebo heslo je neplatné. Začněte přidáním ovládacího prvku popisku pod ovládacím prvku CreateUserWizard nastavení jeho `ID` vlastnost `InvalidUserNameOrPasswordMessage` a jeho `ForeColor` vlastnost `Red`. Vymazání jeho `Text` vlastnost a nastavte jeho `EnableViewState` a `Visible` vlastnosti na hodnotu False.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample7.aspx)]

Dále vytvořte obslužnou rutinu události pro ovládací prvek CreateUserWizard `CreatingUser` událostí. Vytvořte obslužnou rutinu události, vyberte ovládací prvek v návrháři a potom v okně Vlastnosti. Odtud klikněte na na ikonu blesku a potom dvakrát klikněte na příslušnou událost k vytvoření obslužné rutiny události.

Přidejte následující kód, který `CreatingUser` obslužné rutiny události:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample8.cs)]

Všimněte si, že uživatelské jméno a heslo, které zadáte v ovládacím prvku CreateUserWizard jsou k dispozici prostřednictvím jeho [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) a [ `Password` vlastnosti](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)v uvedeném pořadí. Používáme tyto vlastnosti ve výše uvedené obslužné rutiny události k určení, zda zadané uživatelské jméno obsahuje počáteční ani koncové mezery a určuje, zda uživatelské jméno nachází v rámci heslo. Pokud je splněna některá z těchto podmínek, zobrazí se chybová zpráva v `InvalidUserNameOrPasswordMessage` popisek a obslužná rutina události `e.Cancel` je nastavena na `true`. Pokud `e.Cancel` je nastavena na `true`, CreateUserWizard zkratům jejího pracovního postupu, efektivně rušení procesu vytvoření účtu uživatele.

Snímek obrazovky znázorňuje obrázek 15 `CreatingUserAccounts.aspx` když uživatel zadá uživatelské jméno s úvodní mezery.


[![Uživatelská jména u úvodní a koncové mezery nejsou povolené.](creating-user-accounts-cs/_static/image44.png)](creating-user-accounts-cs/_static/image43.png)

**Obrázek 15**: nejsou povolené. uživatelská jména u úvodní a koncové mezery ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-user-accounts-cs/_static/image45.png))


> [!NOTE]
> Vidíte příklad použití ovládacího prvku CreateUserWizard `CreatedUser` událost v *<a id="_msoanchor_11"> </a> [ukládání Další informace o uživateli](storing-additional-user-information-cs.md)* kurzu.


## <a name="summary"></a>Souhrn

`Membership` Třídy `CreateUser` metoda vytvoří nový uživatelský účet v rámci členství. Dělá to tak delegování volání do nakonfigurovaného zprostředkovatele členství. V případě třídy `SqlMembershipProvider`, `CreateUser` metoda přidá záznam `aspnet_Users` a `aspnet_Membership` databázových tabulek.

Zatímco nové uživatelské účty mohou být vytvořeny prostřednictvím kódu programu (jak jsme viděli v kroku 5), je jednodušší a rychlejší přístup pomocí ovládacího prvku CreateUserWizard. Tento ovládací prvek vykreslí vícekrokového uživatelského rozhraní pro shromažďování informací o uživateli a vytvoření nového uživatele v rámci členství. Na pozadí tohoto ovládacího prvku používá stejná `Membership.CreateUser` způsob, jak prozkoumat v kroku 5, ale ovládací prvek uživatelského rozhraní validačních ovládacích prvků, vytvoří a reaguje na chyby při vytváření účtu uživatele bez nutnosti psát ikonu kódu.

V tuto chvíli máme funkci v místo, kde můžete vytvářet nové uživatelské účty. Ale na přihlašovací stránku stále ověřování proti těchto pevně zakódované přihlašovací údaje, zadaný v druhé části kurzu. V <a id="_msoanchor_12"> </a> [další kurz](validating-user-credentials-against-the-membership-user-store-cs.md) aktualizujeme `Login.aspx` k ověření uživatele je zadaná pověření rámec členství.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [`CreateUser` Technická dokumentace](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Přehled ovládacího prvku CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Vytvoření zprostředkovatele mapy webu systém založen souboru](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Vytvoření pomocí ovládacího prvku ASP.NET 2.0 průvodce krok za krokem uživatelského rozhraní](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Zkoumání ASP.NET 2.0 je navigace na webu](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Stránky předlohy a navigace na webu](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Zprostředkovatele mapy webu SQL jste čekali](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k...

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](creating-the-membership-schema-in-sql-server-cs.md)
> [další](validating-user-credentials-against-the-membership-user-store-cs.md)
