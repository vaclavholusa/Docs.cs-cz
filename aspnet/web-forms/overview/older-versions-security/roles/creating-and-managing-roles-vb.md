---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
title: Vytváření a Správa rolí (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Tento kurz zkoumá kroky potřebné ke konfiguraci rozhraní role. Pod vytváříme webové stránky, vytvářet a odstraňovat role.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 83af9f5f-9a00-4f83-8afc-e98bdd49014e
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
msc.type: authoredcontent
ms.openlocfilehash: e51fa6de3d2fe7b5c9cd84900d154070eb1960b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754540"
---
<a name="creating-and-managing-roles-vb"></a>Vytváření a Správa rolí (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.09.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_vb.pdf)

> Tento kurz zkoumá kroky potřebné ke konfiguraci rozhraní role. Pod vytváříme webové stránky, vytvářet a odstraňovat role.


## <a name="introduction"></a>Úvod

V <a id="_msoanchor_1"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurzu jsme zvažovali omezit určitým uživatelům sadu stránek pomocí autorizace adres URL a prozkoumali jste ji deklarativní a programové techniky pro úpravu funkcí stránky technologie ASP.NET na základě hostujícími uživatele. Udělení oprávnění pro přístup stránky nebo funkce na základě uživatele uživatelů, ale může být připomínající údržby ve scénářích tam, kde existuje mnoho uživatelské účty nebo při změně oprávnění uživatelů často. Kdykoli uživatel získá nebo ztratí oprávnění k provedení určitého úkolu, správce je potřeba aktualizovat příslušné autorizačních pravidel adres URL, deklarativní značek a kódu.

Obvykle pomáhá klasifikovat uživatele do skupin nebo *role* a pak použít oprávnění na základě rolí role. Například většiny webových aplikací mají určité sady stránek neboli úlohy, které jsou vyhrazené jenom pro správce. Pomocí technik získané v *autorizace na základě uživatele* kurz, doporučujeme přidat odpovídající autorizačních pravidel adres URL, deklarativní značek a kódu tak, aby zadané uživatelské účty k provádění úloh správy. Ale pokud byl přidán nový správce nebo správce existující potřeba pro její administrativní oprávnění odvolat, musíme se vraťte a aktualizovat konfigurační soubory a webové stránky. S rolemi ale jsme může vytvořit roli s názvem Správci a přiřaďte důvěryhodného uživatele k roli Administrators. Dále doporučujeme přidat odpovídající autorizačních pravidel adres URL, deklarativní a kód umožňující role Správci nástroje k provádění různých administrativních úloh. Pomocí této infrastruktury na místě je snadné – stačí včetně nebo odebrání uživatele z role správce přidávání nových správců k webu nebo odstranění existující aplikace. Žádné konfigurace, deklarativní nebo změny kódu jsou nezbytné.

Technologie ASP.NET nabízí rozhraní role pro definování rolí a jejich přidružení k uživatelské účty. S použitím rozhraní framework role můžeme vytvářet a odstraňovat rolí, přidejte uživatele nebo odebrat uživatele z role, určit sadu uživatelů, kteří patří do určité role a zjistit, zda uživatel patří do určité role. Jakmile rozhraní role není nakonfigurovaná, můžeme omezit přístup ke stránkám na základě rolí role pomocí autorizačních pravidel adres URL a zobrazit nebo skrýt další informace nebo funkce na stránku na základě rolí pro aktuálně přihlášeného uživatele.

Tento kurz zkoumá kroky potřebné ke konfiguraci rozhraní role. Pod vytváříme webové stránky, vytvářet a odstraňovat role. V <a id="_msoanchor_2"> </a> [ *přiřazení rolí uživatelům* ](assigning-roles-to-users-vb.md) kurzu se podíváme na tom, jak přidávat a odebírat uživatele z role. A <a id="_msoanchor_3"> </a> [ *autorizace na základě rolí* ](role-based-authorization-vb.md) kurzu uvidíme, jak omezit přístup ke stránkám na základě rolí role spolu s jak upravit v závislosti funkce stránky hostujícími uživatelské role. Pusťme se do práce!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Přidání nové stránky ASP.NET

V tomto kurzu a další dvě jsme se zkoumání různých funkcí souvisejících s rolí a funkcí. Potřebujeme sérii stránek ASP.NET k implementaci témata prozkoumat v rámci těchto kurzů. Pojďme vytvořit tyto stránky a aktualizaci mapy webu.

Začněte tím, že vytvoříte novou složku v projektu s názvem `Roles`. V dalším kroku přidejte čtyři nové stránky ASP.NET do `Roles` složky propojení s každou stránku `Site.master` stránky předlohy. Název stránky:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Průzkumník řešení vašeho projektu v tomto okamžiku by měl vypadat podobně jako obrazovky je vidět na obrázku 1.


[![Čtyři nové stránky byly přidány do složky role](creating-and-managing-roles-vb/_static/image2.png)](creating-and-managing-roles-vb/_static/image1.png)

**Obrázek 1**: čtyři nové stránky byly přidány do `Roles` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image3.png))


Jednotlivé stránky, v tomto okamžiku má dva ovládací prvky obsahu, jeden pro každou z prvků ContentPlaceHolder na hlavní stránce: `MainContent` a `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample1.aspx)]

Vzpomeňte si, že `LoginContent` ContentPlaceHolder jeho výchozí značky zobrazí odkaz na přihlášení nebo odhlášení uživatele na webu, v závislosti na tom, jestli je uživatel ověřený. Přítomnost `Content2` obsahu ovládacího prvku do stránky ASP.NET, ale přepíše kódu výchozí hlavní stránky. Jak jsme probírali v <a id="_msoanchor_4"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-vb.md) kurzu přepisuje výchozí značky jsou užitečné v stránky, kde jsme nechcete zobrazit související s přihlášením Možnosti v levém sloupci.

Pro tyto čtyři stránky, ale má být zobrazena na hlavní stránce výchozí značky `LoginContent` ContentPlaceHolder. Proto odebrat deklarativní `Content2` ovládacího prvku obsahu. Až to uděláte, všechny čtyři stránky značek by měl obsahovat pouze jeden ovládací prvek obsahu.

A konečně, můžeme aktualizovat mapy webu (`Web.sitemap`) zahrnout tyto nové webové stránky. Přidejte následující kód XML po `<siteMapNode>` jsme přidali kurzy členství.

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample2.xml)]

Pomocí mapy webu, aktualizovat přejděte na web prostřednictvím prohlížeče. Jak je vidět na obrázku 2, navigaci na levé straně teď obsahuje položky pro role kurzy.


[![Čtyři nové stránky byly přidány do složky role](creating-and-managing-roles-vb/_static/image5.png)](creating-and-managing-roles-vb/_static/image4.png)

**Obrázek 2**: čtyři nové stránky byly přidány do `Roles` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Krok 2: Zadání a konfigurace poskytovatele rozhraní Framework role

Jako jsou členství v rámci rozhraní role propojitelnosti podle modelu poskytovatele. Jak je popsáno v <a id="_msoanchor_5"> </a> [ *Základy zabezpečení a podpora ASP.NET* ](../introduction/security-basics-and-asp-net-support-vb.md) kurzu rozhraní .NET Framework se dodává s tři předdefinované role zprostředkovatele: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), a [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). V této sérii kurzů se zaměřuje na `SqlRoleProvider`, který používá databázi serveru Microsoft SQL Server jako úložiště rolí.

Na pozadí rozhraní role a `SqlRoleProvider` pracovat stejně jako rozhraní členství a `SqlMembershipProvider`. Rozhraní .NET Framework obsahuje `Roles` třída, která slouží jako rozhraní API pro role rozhraní framework. `Roles` Sdílí metody, jako je třída `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`a tak dále. Při vyvolání jedné z těchto metod `Roles` deleguje třída volání konfigurovaný poskytovatel. `SqlRoleProvider` Pracuje s tabulkami konkrétní role (`aspnet_Roles` a `aspnet_UsersInRoles`) v odpovědi.

Chcete-li použít `SqlRoleProvider` zprostředkovatele v naší aplikaci, potřebujeme k určení, co Pokud chcete použít jako úložiště databáze. `SqlRoleProvider` Očekává, že úložiště Zadaná role má určitá databázových tabulek, zobrazení a uložených procedur. Tyto objekty požadované databáze můžete přidat pomocí [ `aspnet_regsql.exe` nástroj](https://msdn.microsoft.com/library/ms229862.aspx). V tuto chvíli jsme už máte databázi se schématem, které jsou nezbytné pro `SqlRoleProvider`. Zpátky <a id="_msoanchor_6"> </a> [ *vytvoření schématu členství v SQL serveru* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) kurzu jsme vytvořili databázi s názvem `SecurityTutorials.mdf` a použít `aspnet_regsql.exe` pro přidání aplikace služby, které zahrnuty databázové objekty nezbytné podle `SqlRoleProvider`. Proto potřebujeme k informování rozhraní framework role povolit podporu role a použít `SqlRoleProvider` s `SecurityTutorials.mdf` databáze jako úložiště rolí.

Role rozhraní se konfiguruje prostřednictvím `<roleManager>` elementu v aplikaci prvku `Web.config` souboru. Podpora role je ve výchozím nastavení zakázána. Ho Pokud chcete povolit, je nutné nastavit [ `<roleManager>` ](https://msdn.microsoft.com/library/ms164660.aspx) elementu `enabled` atribut `true` takto:

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample3.xml)]

Ve výchozím nastavení, mají všechny webové aplikace zprostředkovatele rolí s názvem `AspNetSqlRoleProvider` typu `SqlRoleProvider`. Tento výchozí zprostředkovatel je registrován v `machine.config` (umístěný ve `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample4.xml)]

Poskytovatele `connectionStringName` atribut určuje, který se používá úložiště rolí. `AspNetSqlRoleProvider` Poskytovatele nastaví tento atribut na `LocalSqlServer`, který je definován také v `machine.config` a body, ve výchozím nastavení se k databázi SQL Server 2005 Express Edition `App_Data` složku s názvem `aspnet.mdf`.

V důsledku toho pokud jsme bez zadání jakékoli informace o poskytovateli v naší aplikaci jednoduše povolit rozhraní framework role `Web.config` soubor, aplikace používá zprostředkovatel rolí výchozí registrované `AspNetSqlRoleProvider`. Pokud `~/App_Data/aspnet.mdf` databáze buď neexistuje, automaticky ji vytvoříte a přidáte schématu aplikací služby modulu runtime ASP.NET. Nechceme však použít `aspnet.mdf` databáze; místo toho chceme použít `SecurityTutorials.mdf` databáze, který jsme už vytvořili a přidali schématu služby aplikace. Tato změna lze provést jedním ze dvou způsobů:

- <strong>Zadejte hodnotu</strong><strong>`LocalSqlServer`</strong><strong>název připojovacího řetězce v</strong><strong>`Web.config`</strong><strong>.</strong> Přepsáním `LocalSqlServer` hodnotu název připojovacího řetězce v `Web.config`, můžeme použít výchozí zaregistrovaný zprostředkovatel rolí (`AspNetSqlRoleProvider`) a jeho správně pracovat `SecurityTutorials.mdf` databáze. Další informace o této techniky najdete v tématu [Scott Guthrie](https://weblogs.asp.net/scottgu/)pro blogový příspěvek [konfigurace technologie ASP.NET 2.0 aplikačními službami, abyste pomocí SQL Server 2000 nebo SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Přidat nového poskytovatele registrovaného typu</strong><strong>`SqlRoleProvider`</strong><strong>a nakonfigurovat jeho</strong><strong>`connectionStringName`</strong><strong>nastavení tak, aby odkazoval</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>databáze.</strong> Jedná se o postup I doporučené a použít v <a id="_msoanchor_7"> </a> [ *vytvoření schématu členství v SQL serveru* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) kurzu a je využívána v tomto kurzu také přístup.

Přidejte následující kód konfigurace rolí k `Web.config` souboru. Tento kód zaregistruje nový zprostředkovatel s názvem `SecurityTutorialsSqlRoleProvider.`

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample5.xml)]

Výše uvedené značky definují `SecurityTutorialsSqlRoleProvider` jako výchozího zprostředkovatele (prostřednictvím `defaultProvider` atribut `<roleManager>` element). Také nastaví `SecurityTutorialsSqlRoleProvider`společnosti `applicationName` nastavení `SecurityTutorials`, což je stejná `applicationName` nastavení používá zprostředkovatel členství (`SecurityTutorialsSqlMembershipProvider`). Zatímco není tady, zobrazené [ `<add>` element](https://msdn.microsoft.com/library/ms164662.aspx) pro `SqlRoleProvider` může také obsahovat `commandTimeout` atribut určit dobu trvání časového limitu databáze během několika sekund. Výchozí hodnota je 30.

Pomocí této konfigurace značek na místě budeme připravení začít používat funkce role v rámci naší aplikace.

> [!NOTE]
> Výše uvedené značek konfigurace znázorňuje použití `<roleManager>` elementu `enabled` a `defaultProvider` atributy. Existuje mnoho dalších atributů, které ovlivňují, jak rozhraní role přidruží informace o rolích na základě uživatele uživatelů. Prozkoumáme tohoto nastavení <a id="_msoanchor_8"> </a> [ *autorizace na základě rolí* ](role-based-authorization-vb.md) kurzu.


## <a name="step-3-examining-the-roles-api"></a>Krok 3: Zkoumání rozhraní API rolí

Funkce rolí rozhraní jsou dostupné prostřednictvím [ `Roles` třída](https://msdn.microsoft.com/library/system.web.security.roles.aspx), který obsahuje třináct sdílené metody pro provádění operací na základě rolí. Když podíváme na vytváření a odstraňování role v krocích 4 a 6 budeme používat [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) a [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) metody, které přidat nebo odebrat roli ze systému.

Chcete-li získat seznam všech rolí v systému, použijte [ `GetAllRoles` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (viz krok 5). [ `RoleExists` Metoda](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) vrátí logickou hodnotu označující, zda zadaná role existuje.

V dalším kurzu prozkoumáme spojení uživatelů s rolemi. `Roles` Třídy [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), a [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) metody přidat jeden nebo více uživatelů k jedné nebo více rolím. Chcete-li odebrat uživatele z role, použijte [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), nebo [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) metody.

V <a id="_msoanchor_9"> </a> [ *autorizace na základě rolí* ](role-based-authorization-vb.md) kurzu se podíváme na způsoby, jak programově zobrazit nebo skrýt funkce na základě role aktuálně přihlášeného uživatele. K tomu můžeme použít třídy Role [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), nebo [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) metody.

> [!NOTE]
> Mějte na paměti, která je vyvolána vždy jeden z těchto metod, `Roles` deleguje třída volání konfigurovaný poskytovatel. V našem případě to znamená, že volání se posílá `SqlRoleProvider`. `SqlRoleProvider` Pak provede operaci příslušnou databázi podle volanou metodu. Například kód `Roles.CreateRole("Administrators")` vede `SqlRoleProvider` provádění `aspnet_Roles_CreateRole` uloženou proceduru, která vloží nový záznam do `aspnet_Roles` tabulku s názvem Správci.


Zbývající část tohoto kurzu zkoumá pomocí `Roles` třídy `CreateRole`, `GetAllRoles`, a `DeleteRole` metody pro správu role v systému.

## <a name="step-4-creating-new-roles"></a>Krok 4: Vytvoření nové role

Role nabízí způsob, jak libovolně skupiny uživatelů a většinou je toto seskupení používá pro pohodlnější způsob, jak aplikovat autorizační pravidla. Ale pokud chcete používat role jako mechanismus ověřování, musíme nejprve definovat, jaké role existovat v aplikaci. ASP.NET bohužel neobsahuje CreateRoleWizard ovládacího prvku. Chcete-li přidat nové role potřebné k vytvoření vhodného uživatelského rozhraní a volat rozhraní API rolí sami. Dobrou zprávou je, že to je velmi snadné.

> [!NOTE]
> Neplatí žádné CreateRoleWizard webový ovládací prvek, je [nástroje pro správu webu technologie ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), což je místní aplikace v ASP.NET navržené tak, aby vám pomůže s zobrazení a správa konfigurace webové aplikace. Ale nejsem velký fanda nástroje pro správu webu technologie ASP.NET pro dva důvody. Nejprve je o něco obsahujícím chyby a činnost koncového uživatele ponechá být požadovaného mnoho dalších. Za druhé nástroje pro správu webu technologie ASP.NET slouží pouze pracovat místně, to znamená, že budete muset sestavit vlastní role správu webových stránek, když potřebujete vzdáleně spravovat role na živý web. Dva z těchto důvodů v tomto kurzu, kdy se soustředí na vytváření roli potřebné nástroje pro správu na webové stránce, spíše než spoléhání se na nástroje pro správu webu technologie ASP.NET.


Otevřít `ManageRoles.aspx` stránku `Roles` složky a přidat textové pole a tlačítko webový ovládací prvek na stránce. Nastavení ovládacího prvku textového pole `ID` vlastnost `RoleName` a na tlačítko `ID` a `Text` vlastností `CreateRoleButton` a vytvořit roli, v uvedeném pořadí. Na stránce deklarativní v tomto okamžiku by měl vypadat nějak takto:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample6.aspx)]

Potom dvakrát klikněte na panel `CreateRoleButton` tlačítko ovládacího prvku do návrháře k vytvoření `Click` obslužné rutiny události a potom přidejte následující kód:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample7.vb)]

Výše uvedený kód spustí přiřazením zadaný v název oříznutý role `RoleName` textového pole `newRoleName` proměnné. Dále `Roles` třídy `RoleExists` metoda je volána k určení, zda role `newRoleName` v systému již existuje. Pokud roli buď neexistuje, vytvoří se prostřednictvím volání `CreateRole` metody. Pokud `CreateRole` metodě se předává název role, která již existuje v systému, `ProviderException` je vyvolána výjimka. To je důvod, proč kód nejprve zkontroluje, ujistěte se, že role již neexistuje v systému před voláním `CreateRole`. `Click` Obslužná rutina události dojde k závěru zrušením navýšení kapacity `RoleName` textového `Text` vlastnost.

> [!NOTE]
> Asi vás zajímá co se stane, pokud uživatel nemá zadejte libovolnou hodnotu do `RoleName` textového pole. Pokud hodnota předaná do `CreateRole` je metoda `Nothing` nebo prázdný řetězec, výjimka je vyvolána. Podobně pokud je název role obsahuje čárku je vyvolána výjimka. Na stránce v důsledku toho by měl obsahovat validačních ovládacích prvků k zajištění, aby uživatel zadal roli a zda neobsahuje žádné čárkami. Opuštění jako cvičení pro čtečku.


Umožňuje vytvořit roli s názvem Správci. Přejděte `ManageRoles.aspx` stránce prostřednictvím prohlížeče, do textového pole zadejte správce (viz obrázek 3) a potom klikněte na tlačítko Vytvořit roli.


[![Umožňuje vytvořit roli správce](creating-and-managing-roles-vb/_static/image8.png)](creating-and-managing-roles-vb/_static/image7.png)

**Obrázek 3**: umožňuje vytvořit roli Administrators ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image9.png))


Co se stane? Vyvolá zpětné volání, ale neexistuje vizuální upozornění, která ve skutečnosti role byla přidána do systému. Aktualizujeme tuto stránku v kroku 5 a zahrnují vizuální zpětnou vazbu. Prozatím se však můžete ověřit, že role byla vytvořena tak, že přejdete `SecurityTutorials.mdf` databáze a zobrazení dat z `aspnet_Roles` tabulky. Jak ukazuje obrázek 4 `aspnet_Roles` tabulka obsahuje záznam pro roli správce právě přidali.


[![Aspnet_Roles tabulka obsahuje řádek pro správce](creating-and-managing-roles-vb/_static/image11.png)](creating-and-managing-roles-vb/_static/image10.png)

**Obrázek 4**: `aspnet_Roles` tabulka obsahuje řádek pro správce ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Krok 5: Zobrazení role v systému

Můžeme rozšířit `ManageRoles.aspx` stránky, aby zahrnovala seznam aktuální role v systému. K tomu přidat ovládací prvek GridView na stránku a nastavit jeho `ID` vlastnost `RoleList`. V dalším kroku přidejte metodu do třídy modelu code-behind na stránce s názvem `DisplayRolesInGrid` pomocí následujícího kódu:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample8.vb)]

`Roles` Třídy `GetAllRoles` metoda vrátí všechny role v systému jako pole řetězců. Tato pole řetězců je pak vázán na prvku GridView. Aby bylo možné vytvořit vazbu seznamu rolí do prvku GridView, při prvním načtení stránky, musíme volání `DisplayRolesInGrid` metodu na stránce `Page_Load` obslužné rutiny události. Následující kód volá tuto metodu při první návštěvě stránky, ale ne na následné zpětného odeslání.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample9.vb)]

S tímto kódem na místě navštivte stránku prostřednictvím prohlížeče. Jak je vidět na obrázku 5, měli byste vidět mřížku s jedním sloupcem označené položky. Mřížka obsahuje řádek pro roli správce, kterou jsme přidali v kroku 4.


[![GridView zobrazí role v jednom sloupci](creating-and-managing-roles-vb/_static/image14.png)](creating-and-managing-roles-vb/_static/image13.png)

**Obrázek 5**: prvku GridView zobrazí role v jednom sloupci ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image15.png))


GridView zobrazí jedinou sloupec s názvem položky, protože prvku GridView `AutoGenerateColumns` je nastavena na hodnotu True (výchozí), což způsobí, že prvku GridView. Chcete-li automaticky vytvořit sloupec pro každou vlastnost v jeho `DataSource`. Pole má jednu vlastnost, která představuje prvků v poli, takže jeden sloupec v prvku GridView.

Při zobrazení dat ovládacím prvkem GridView, raději chcete explicitně definovat sloupce a nenechat je implicitně generovaný prvku GridView. Explicitně definovat sloupce je mnohem jednodušší zobrazení dat, změnit uspořádání sloupců a provádět další běžné úlohy. Proto můžeme aktualizovat prvku GridView deklarativní tak, aby její sloupce nejsou explicitně definovány.

Začněte tím, že nastavení prvku GridView `AutoGenerateColumns` vlastnost na hodnotu False. Dále přidejte TemplateField do mřížky, nastavte jeho `HeaderText` vlastnost k rolím a nakonfigurovat jeho `ItemTemplate` tak, aby zobrazil obsah pole. K tomu přidat ovládací prvek webového popisek s názvem `RoleNameLabel` k `ItemTemplate` a vytvořit vazbu jeho `Text` vlastnost `Container.DataItem.`

Tyto vlastnosti a `ItemTemplate`jeho obsah lze nastavit pomocí deklarace nebo přes dialogové okno pole prvku GridView a interface upravit šablony. Pole dialogové okno, klikněte na odkaz Upravit sloupce v prvku GridView inteligentních značek. V dalším kroku, zrušte zaškrtnutí políčka automaticky generovat pole nastavit `AutoGenerateColumns` vlastnost na hodnotu False a přidejte do prvku GridView, TemplateField nastavení jeho `HeaderText` vlastnost do Role. Chcete-li definovat `ItemTemplate`na obsah, zvolte možnost Upravit šablony z inteligentních značek v prvku GridView. Přetáhněte ovládací prvek popisek Web na `ItemTemplate`, nastavte jeho `ID` vlastnost `RoleNameLabel`a nastavení jeho vázání dat tak, aby jeho `Text` vlastnost je vázána na `Container.DataItem`.

Bez ohledu na to, jaký přístup je použít, prvku GridView výsledný deklarativní značka by měla vypadat podobně jako následujícím po dokončení.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample10.aspx)]

> [!NOTE]
> Obsah tohoto pole se zobrazí pomocí syntaxe databinding `<%# Container.DataItem %>`. Nad rámec tohoto kurzu je důkladné popis Proč tato syntaxe je použita při zobrazení obsahu pole vázána na prvku GridView. Další informace o této věci, předložit [skalární pole vazby na ovládací prvek webových dat](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


V současné době `RoleList` GridView je vázaný jenom na seznamu rolí, při první návštěvě stránky. Potřebujeme aktualizovat mřížky pokaždé, když se přidá novou roli. Jak toho dosáhnout, aktualizujte `CreateRoleButton` tlačítka `Click` tak, že volá obslužná rutina události `DisplayRolesInGrid` metodu, pokud se vytvoří novou roli.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample11.vb)]

Teď, když uživatel přidá novou roli `RoleList` GridView ukazuje roli právě přidané na zpětné volání, poskytuje vizuální zpětnou vazbu, že role se úspěšně vytvořil. Pro znázornění, přejděte `ManageRoles.aspx` stránce prostřednictvím prohlížeče a přidejte roli s názvem správců. Po kliknutí na tlačítko Vytvořit roli, bude následovat zpětné volání a mřížce se aktualizuje a zahrnují správcích a také novou roli vedoucí.


[![Role správců se přidala](creating-and-managing-roles-vb/_static/image17.png)](creating-and-managing-roles-vb/_static/image16.png)

**Obrázek 6**: Role správců se přidala ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Krok 6: Odstranění role

V tomto okamžiku můžete uživatele vytvořit novou roli a zobrazit všechny existující role z `ManageRoles.aspx` stránky. Můžeme povolit uživatelům také odstranění rolí. `Roles.DeleteRole` Metoda má dvě přetížení:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Odstraní roli *roleName*. Pokud role obsahuje jeden nebo více členů, je vyvolána výjimka.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Odstraní roli *roleName*. Pokud *throwOnPopulateRole* je `True`, pak je vyvolána výjimka, pokud role obsahuje jeden nebo více členů. Pokud *throwOnPopulateRole* je `False`, role je odstraněna, zda obsahuje všechny členy, nebo ne. Interně `DeleteRole(roleName)` volání metody `DeleteRole(roleName, True)`.

`DeleteRole` Metoda také vyvolá výjimku, pokud *roleName* je `Nothing` nebo prázdný řetězec nebo, pokud *roleName* obsahuje čárku. Pokud *roleName* neexistuje v systému, `DeleteRole` selže tiše, bez vyvolání výjimky.

Můžeme rozšířit v prvku GridView `ManageRoles.aspx` zahrnout odstranění tlačítka, který po kliknutí na odstraní vybranou roli. Začněte přidáním tlačítko pro odstranění k prvku GridView. dialogové okno pole a tlačítko pro odstranění, které se nachází v rámci CommandField možnost přidání. Ujistěte se, odstraňte sloupec úplně vlevo tlačítko a nastavte jeho `DeleteText` vlastnost odstranit roli.


[![Přidejte tlačítko pro odstranění RoleList GridView](creating-and-managing-roles-vb/_static/image20.png)](creating-and-managing-roles-vb/_static/image19.png)

**Obrázek 7**: přidat tlačítko Odstranit `RoleList` ovládacího prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image21.png))


Po přidání tlačítko pro odstranění, deklarativní prvku GridView by měl vypadat nějak takto:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample12.aspx)]

Dále vytvořte obslužnou rutinu události pro prvku GridView `RowDeleting` událostí. Toto je událost, která je vyvolána na zpětné volání, když dojde ke kliknutí na tlačítko odstranit roli. Přidejte následující kód do obslužné rutiny události.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample13.vb)]

Kód spustí programově odkazováním `RoleNameLabel` ovládací prvek v řádku, jehož tlačítko odstranit roli došlo ke kliknutí na webu. `Roles.DeleteRole` Potom je volána metoda, předejte `Text` z `RoleNameLabel` a `False`, a tím odstranění role bez ohledu na to, zda jsou všechny uživatele přidružené k roli. Nakonec `RoleList` GridView se aktualizují tak, aby role právě odstraněné již nebude zobrazovat v mřížce.

> [!NOTE]
> Tlačítko odstranit roli nevyžaduje, aby jakýkoli druh potvrzení od uživatele před odstraněním role. Jedním z nejjednodušších způsobů k potvrzení akce je prostřednictvím dialogového okna potvrdit na straně klienta. Další informace o této techniky najdete v tématu [přidání Client-Side potvrzení při odstraňování](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


## <a name="summary"></a>Souhrn

Mnoho webových aplikací mají určité autorizační pravidla nebo funkci úrovni stránky, která je dostupná jenom pro určité třídy uživatelů. Například může být sada webových stránek, ke kterým přístup jenom správci. Místo definování tato pravidla autorizace na základě uživatele podle uživatelů, je často další užitečné k definování pravidel založený na roli. To znamená, nikoli explicitně umožnit uživatelům Scott a Jisun přístup pro správu webových stránek, snadněji spravovatelné přístup je tak, aby povolovala členy rolí správce pro přístup k tyto stránky a pak k označení Scott a Jisun jako uživatelé, kteří patří do Role správce.

Rozhraní rolí umožňuje snadno vytvořit a spravovat role. V tomto kurzu jsme se zaměřili na tom, jak nakonfigurovat rozhraní role používat `SqlRoleProvider`, který používá databázi serveru Microsoft SQL Server jako úložiště rolí. Jsme také vytvořili webovou stránku, která obsahuje seznam existujících rolí v systému a umožňuje nové role, který se má vytvořit a odstranit existující. V následujících kurzech uvidíme, jak přiřadit uživatele k rolím a jak se autorizace na základě rolí.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Zkoumání ASP.NET 2.0 pro členství, role a profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Postupy: Použití správce rolí technologie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Role zprostředkovatele](https://msdn.microsoft.com/library/aa478950.aspx)
- [Vlastní nástroj pro správu webu se zajištěním provozu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Technická dokumentace ke službě `<roleManager>` – Element](https://msdn.microsoft.com/library/ms164660.aspx)
- [Pomocí členství a Role Správce rozhraní API](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu zahrnují Alicja Maziarz, Suchi Banerjee a Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](role-based-authorization-cs.md)
> [další](assigning-roles-to-users-vb.md)
