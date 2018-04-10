---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
title: Vytvoření a správy rolí (VB) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu prověří kroky nezbytné pro konfiguraci role framework. Následující, vytvoříme vytvářet a odstraňovat role webové stránky.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83af9f5f-9a00-4f83-8afc-e98bdd49014e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
msc.type: authoredcontent
ms.openlocfilehash: 75ca9b1c36f9a74d755ef05717f03d139d0b29ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
---
<a name="creating-and-managing-roles-vb"></a>Vytvoření a správy rolí (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.09.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_vb.pdf)

> V tomto kurzu prověří kroky nezbytné pro konfiguraci role framework. Následující, vytvoříme vytvářet a odstraňovat role webové stránky.


## <a name="introduction"></a>Úvod

V <a id="_msoanchor_1"> </a> [ *autorizace na základě uživatele* ](../membership/user-based-authorization-vb.md) kurzu jsme hledá pomocí autorizace adres URL omezit určité uživatele ze sady stránek a prozkoumali deklarativní a Programová techniky pro úpravu stránku ASP.NET funkce na základě hostujícími uživatele. Udělení oprávnění pro přístup stránky nebo funkce na základě uživatele uživatelů, ale může stát při důvodů údržby ve scénářích tam, kde existuje mnoho uživatelských účtů nebo při změně oprávnění uživatelů. Vždy, když uživatel získá nebo ztratí oprávnění k provedení určité úlohy, Správce musí aktualizovat příslušný autorizačních pravidel adres URL, deklarativní značek a kódu.

Obvykle pomáhá klasifikovat uživatele do skupiny nebo *role* a pak použít oprávnění na základě rolí role. Například většiny webových aplikací mít určitou sadu stránek nebo úlohy, které jsou vyhrazené jenom pro administrativní uživatele. Pomocí technik se naučili v *autorizace na základě uživatele* kurzu doporučujeme přidat odpovídající autorizačních pravidel adres URL, deklarativní značek a kódu tak, aby zadané uživatelské účty k provádění úloh správy. Ale pokud byl přidán nový správce nebo správce existující potřeba pro jeho práva správce odvoláno, nám vrátit a aktualizovat konfigurační soubory a webové stránky. S rolemi přesto však může vytvořit role s názvem Správci a přiřadit uživatelům důvěryhodné role správců. Dále doporučujeme přidat odpovídající autorizačních pravidel adres URL, deklarativní značek a kódu povolit role správců k provádění různých úloh správy. Pomocí této infrastruktury na místě je jednoduché, včetně nebo odebrání uživatele z role správců přidáním noví správci k webu nebo odebráním stávajících. Žádná konfigurace, deklarativní ani změny kódu jsou nezbytné.

Technologie ASP.NET nabízí rozhraní role pro definování rolí a jejich přidružení k uživatelské účty. S rolí framework jsme můžete vytvářet a odstraňovat role přidejte uživatelům nebo odebrat uživatele z role, určit sadu uživatelů, kteří patří do konkrétní roli a zjistit, zda uživatel patří do určité role. Jakmile byl nakonfigurován rozhraní role, jsme omezit přístup k stránky na základě rolí role pomocí autorizačních pravidel adres URL a zobrazit nebo skrýt další informace nebo funkce na stránce na základě rolí aktuálně přihlášeného uživatele.

V tomto kurzu prověří kroky nezbytné pro konfiguraci role framework. Následující, vytvoříme vytvářet a odstraňovat role webové stránky. V <a id="_msoanchor_2"> </a> [ *přiřazení rolí uživatelů* ](assigning-roles-to-users-vb.md) kurz se podíváme na tom, jak přidat a odebrat uživatele z role. A v <a id="_msoanchor_3"> </a> [ *autorizace na základě rolí* ](role-based-authorization-vb.md) kurzu zjistíme, jak omezit přístup na stránky na základě rolí role společně s postupy upravit podle funkce stránky na hostujícími uživatelské role. Můžeme začít!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Přidání nových stránek ASP.NET

V tomto kurzu a další dvě jsme se prověřují různé funkce související s rolí a funkcí. Potřebujeme řadu stránek ASP.NET implementovat témata zkontrolován v rámci těchto kurzů. Umožňuje vytvořit tyto stránek a aktualizovat mapy webu.

Začněte vytvořením novou složku v projektu s názvem `Roles`. Dál přidejte čtyři nové stránky ASP.NET do `Roles` složky, propojení každou stránku s `Site.master` stránky předlohy. Název stránky:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Vašeho projektu Průzkumníku řešení v tomto okamžiku by mělo vypadat jako na uvedené na obrázku 1 snímku obrazovky.


[![Čtyři nové stránky přidané do složky role](creating-and-managing-roles-vb/_static/image2.png)](creating-and-managing-roles-vb/_static/image1.png)

**Obrázek 1**: čtyři nové stránky přidané `Roles` složky ([Kliknutím zobrazit obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image3.png))


Každé stránce měli v tomto okamžiku dva ovládací prvky obsahu, jeden pro každou stránku předlohy ContentPlaceHolders: `MainContent` a `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample1.aspx)]

Odvolat, který `LoginContent` ContentPlaceHolder na výchozí značka zobrazí odkaz na přihlášení a odhlášení lokality, v závislosti na tom, jestli je uživatel ověřený. Přítomnost `Content2` obsahu ovládacího prvku do stránky ASP.NET, ale přepíše značek výchozí stránky předlohy. Jak již bylo zmíněno <a id="_msoanchor_4"> </a> [ *Přehled ověřování založené na formulářích* ](../introduction/an-overview-of-forms-authentication-vb.md) kurzu přepsání výchozí značka je užitečné v stránky, kde jsme nechcete zobrazit související s přihlášením Možnosti v levém sloupci.

Pro tyto čtyři stránky, ale chceme zobrazit kód výchozí stránky předlohy pro `LoginContent` ContentPlaceHolder. Proto odebrat deklarativní `Content2` obsahu ovládacího prvku. Až to uděláte, všechny čtyři stránce značek by měl obsahovat pouze jeden prvek obsahu.

Nakonec můžeme aktualizovat mapy webu (`Web.sitemap`) zahrnout tyto nové webové stránky. Přidejte následující kód XML po `<siteMapNode>` jsme přidali pro kurzy členství.

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample2.xml)]

S mapy webu aktualizovat přejděte na web prostřednictvím prohlížeče. Jak je vidět na obrázku 2, navigaci na levé straně teď obsahuje položky pro kurzy role.


[![Čtyři nové stránky přidané do složky role](creating-and-managing-roles-vb/_static/image5.png)](creating-and-managing-roles-vb/_static/image4.png)

**Obrázek 2**: čtyři nové stránky přidané `Roles` složky ([Kliknutím zobrazit obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Krok 2: Zadání a konfiguraci Framework zprostředkovatel rolí

Jako rozhraní členství je rozhraní role vytvořené na modelu poskytovatelů. Jak je popsáno v <a id="_msoanchor_5"> </a> [ *Základy zabezpečení a podporu ASP.NET* ](../introduction/security-basics-and-asp-net-support-vb.md) kurzu rozhraní .NET Framework se dodává s tři předdefinované role zprostředkovatele: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), a [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Tato řada kurz se zaměřuje na `SqlRoleProvider`, který používá databázi systému Microsoft SQL Server jako úložiště rolí.

Pod pozadí rozhraní role a `SqlRoleProvider` pracovní stejně jako rozhraní členství a `SqlMembershipProvider`. Obsahuje rozhraní .NET Framework `Roles` třídu, která slouží jako rozhraní API, abyste rozhraní role. `Roles` Třída sdílí metody, třeba `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`, a tak dále. Po vyvolání jednu z těchto metod `Roles` deleguje třída volání nakonfigurovaného zprostředkovatele. `SqlRoleProvider` Pracuje s tabulkami specifickou rolí (`aspnet_Roles` a `aspnet_UsersInRoles`) v odpovědi.

Chcete-li použít `SqlRoleProvider` poskytovatele v naší aplikaci, musíme určit, co Pokud chcete použít jako úložiště databáze. `SqlRoleProvider` Očekává úložišti roli tak, aby měl určité databázových tabulek, zobrazení a uložených procedur. Tyto objekty požadavků databázi lze přidat pomocí [ `aspnet_regsql.exe` nástroj](https://msdn.microsoft.com/library/ms229862.aspx). V tuto chvíli jsme už máte databázi se schématem potřebné pro `SqlRoleProvider`. Zpět v <a id="_msoanchor_6"> </a> [ *vytváření schématu členství v systému SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) kurzu jsme vytvořili databázi s názvem `SecurityTutorials.mdf` a použít `aspnet_regsql.exe` přidat aplikaci služby, které jsou zahrnuty databázové objekty nezbytné pomocí `SqlRoleProvider`. Proto potřebujeme říct rozhraní role podporu role a používat `SqlRoleProvider` s `SecurityTutorials.mdf` databáze jako úložiště rolí.

Rozhraní framework role je nakonfigurovaný pomocí `<roleManager>` element do aplikace `Web.config` souboru. Podpora role je ve výchozím nastavení zakázána. Chcete-li ji povolit, musíte nastavit [ `<roleManager>` ](https://msdn.microsoft.com/library/ms164660.aspx) elementu `enabled` atribut `true` takto:

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample3.xml)]

Ve výchozím nastavení všech webových aplikací mít zprostředkovatele rolí s názvem `AspNetSqlRoleProvider` typu `SqlRoleProvider`. Tento výchozí zprostředkovatel je zaregistrován v `machine.config` (umístěné v `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample4.xml)]

Poskytovatele `connectionStringName` Určuje atribut role úložiště, který se používá. `AspNetSqlRoleProvider` Zprostředkovatele tento atribut nastaví na `LocalSqlServer`, který je také definován v `machine.config` a body, ve výchozím nastavení se k databázi SQL Server 2005 Express Edition v `App_Data` složku s názvem `aspnet.mdf`.

V důsledku toho pokud jednoduše povolíte rozhraní role bez zadání žádné informace o poskytovateli v naší aplikaci `Web.config` soubor, aplikace použije výchozí zaregistrovaný zprostředkovatel rolí, `AspNetSqlRoleProvider`. Pokud `~/App_Data/aspnet.mdf` databáze neexistuje, bude modulem runtime ASP.NET automaticky vytvořit a přidat schématu služby aplikací. Jsme ale nechcete použít `aspnet.mdf` databáze; místo toho chcete použít `SecurityTutorials.mdf` databáze, který jsme už máte vytvořené a přidat schéma služby aplikace tak, aby. Tato úprava se dá udělat v jednom ze dvou způsobů:

- <strong>Zadejte hodnotu</strong><strong>`LocalSqlServer`</strong><strong>název připojovacího řetězce v</strong><strong>`Web.config`</strong><strong>.</strong> Přepsáním `LocalSqlServer` hodnota název připojovacího řetězce v `Web.config`, můžeme použít výchozí zaregistrovaný zprostředkovatel rolí (`AspNetSqlRoleProvider`) a mějte ho správně pracovat `SecurityTutorials.mdf` databáze. Další informace o této technice najdete v tématu [Scott Guthrie](https://weblogs.asp.net/scottgu/)na příspěvku na blogu [konfigurace služby ASP.NET 2.0 pomocí SQL Server 2000 nebo SQL Server 2005 aplikace](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Přidat nového poskytovatele registrované typu</strong><strong>`SqlRoleProvider`</strong><strong>a nakonfigurujte jeho</strong><strong>`connectionStringName`</strong><strong>nastavení tak, aby odkazoval</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>databáze.</strong> Toto je doporučená a použít v přístup <a id="_msoanchor_7"> </a> [ *vytváření schématu členství v systému SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) kurzu a je přístup použiji v tomto kurzu také.

Přidejte následující značku konfiguraci role na `Web.config` souboru. Tento kód zaregistruje nového poskytovatele s názvem `SecurityTutorialsSqlRoleProvider.`

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample5.xml)]

Výše uvedený kód definuje `SecurityTutorialsSqlRoleProvider` jako výchozí zprostředkovatel (prostřednictvím `defaultProvider` atribut `<roleManager>` element). Nastaví taky `SecurityTutorialsSqlRoleProvider`na `applicationName` nastavení `SecurityTutorials`, což je stejný `applicationName` nastavení používá zprostředkovatel členství (`SecurityTutorialsSqlMembershipProvider`). Při není tady, zobrazené [ `<add>` element](https://msdn.microsoft.com/library/ms164662.aspx) pro `SqlRoleProvider` může obsahovat také `commandTimeout` atribut a určit dobu trvání databáze časového limitu v sekundách. Výchozí hodnota je 30.

Pomocí této konfigurace značek na místě jsou připravení začít používat funkce role v rámci naší aplikace.

> [!NOTE]
> Výše uvedený kód konfigurace ilustruje, pomocí `<roleManager>` elementu `enabled` a `defaultProvider` atributy. Existuje několik dalších atributů, které mají vliv jak rozhraní role přidruží informace o rolích na základě uživatele uživatelů. Vyzkoušíme tohoto nastavení <a id="_msoanchor_8"> </a> [ *autorizace na základě rolí* ](role-based-authorization-vb.md) kurzu.


## <a name="step-3-examining-the-roles-api"></a>Krok 3: Prozkoumání rozhraní API rolí

Funkce rolí rozhraní je zveřejněna prostřednictvím [ `Roles` – třída](https://msdn.microsoft.com/library/system.web.security.roles.aspx), který obsahuje třináct sdílené metody pro provádění operací na základě rolí. Když se podíváme na vytvoření nebo odstranění rolí v kroky 4 a 6 budeme používat [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) a [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) metody, které přidat nebo odebrat roli ze systému.

Chcete-li získat seznam všech rolí v systému, použijte [ `GetAllRoles` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (viz krok 5). [ `RoleExists` Metoda](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) vrací logickou hodnotu udávající, zda zadaná role existuje.

V dalším kurzu vyzkoušíme jak spojovat uživatele s rolí. `Roles` Třídy [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), a [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) metody přidat jeden nebo více uživatelů k jedné nebo více rolí. Chcete-li odebrat uživatele z role, použijte [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), nebo [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) metody.

V <a id="_msoanchor_9"> </a> [ *autorizace na základě rolí* ](role-based-authorization-vb.md) kurzu se podíváme na způsoby, jak programově zobrazit nebo skrýt funkce na základě role aktuálně přihlášeného uživatele. K tomu můžeme použít třídu Role [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), nebo [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) metody.

> [!NOTE]
> Mějte na paměti, která je volána, kdykoli, jednu z těchto metod, `Roles` deleguje třída volání nakonfigurovaného zprostředkovatele. V našem případě to znamená, že volání je odesílána `SqlRoleProvider`. `SqlRoleProvider` Pak provede operaci příslušné databáze založené na vyvolanou metodu. Například kód `Roles.CreateRole("Administrators")` výsledkem `SqlRoleProvider` provádění `aspnet_Roles_CreateRole` uložené procedury, která vloží nový záznam do `aspnet_Roles` tabulku s názvem Správci.


Zbývající část tohoto kurzu zjistí pomocí `Roles` třídy `CreateRole`, `GetAllRoles`, a `DeleteRole` metody pro správu role v systému.

## <a name="step-4-creating-new-roles"></a>Krok 4: Vytvoření nové role

Role nabízejí způsob, jak nahodile skupiny uživatelů a nejčastěji toto seskupení se používá pro více pohodlný způsob, jak použít autorizačních pravidel. Ale pokud chcete používat jako mechanismus autorizace role musíme nejprve definovat, jaké role existovat v aplikaci. ASP.NET bohužel neobsahuje CreateRoleWizard ovládacího prvku. Chcete-li přidat nové role je potřeba vytvořit vhodné uživatelské rozhraní a volat rozhraní API rolí označována. Dobrá zpráva je, že se jedná o velmi snadno dosáhnout.

> [!NOTE]
> Když neexistuje žádné CreateRoleWizard webové ovládací prvek, je [nástroj Správa webu ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), což je místní aplikace ASP.NET pomoct s zobrazování a správu konfigurace webové aplikace. Ale nejsem big ventilátor nástroje pro správu webu ASP.NET dvou důvodů. Nejprve je trochu buggy a činnost koncového uživatele opustí mnoho k být potřeby. Druhý nástroj Správa webu ASP.NET je určena pro pouze místně, což znamená, že bude nutné vytvářet vlastní role správy webové stránky, pokud potřebujete ke vzdálené správě rolí na živý web. Dva z těchto důvodů v tomto kurzu a další se soustředí na vytváření potřebné role nástroje pro správu na webové stránce, namísto spoléhání na nástroj Správa webu ASP.NET.


Otevřete `ManageRoles.aspx` stránku `Roles` složky a přidat textové pole a tlačítko webový ovládací prvek na stránku. Nastavení ovládacího prvku TextBox `ID` vlastnost `RoleName` a na tlačítko `ID` a `Text` vlastnosti, které chcete `CreateRoleButton` a vytvořit roli, v uvedeném pořadí. V tomto okamžiku vaší stránky deklarativní by měl vypadat takto:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample6.aspx)]

V dalším kroku klikněte dvakrát na `CreateRoleButton` tlačítko ovládacího prvku v Návrháři k vytvoření `Click` obslužné rutiny události a poté přidejte následující kód:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample7.vb)]

Výše uvedený kód spustí přiřazením zadaný v název oříznutý role `RoleName` textové pole k `newRoleName` proměnné. Dále `Roles` třídy `RoleExists` metoda je volána k určení, zda role `newRoleName` v systému již existuje. Pokud roli neexistuje, vytvoří se prostřednictvím volání `CreateRole` metoda. Pokud `CreateRole` metodě se předává název role, který již existuje v systému, `ProviderException` je vyvolána výjimka. To je důvod, proč kód nejdřív zkontroluje, ujistěte se, že role již neexistuje v systému před voláním `CreateRole`. `Click` Obslužné rutiny události se ukončí vymazání `RoleName` textové pole na `Text` vlastnost.

> [!NOTE]
> Asi vás zajímá, co se stane Pokud uživatel není zadat jakoukoli hodnotu do `RoleName` textové pole. Pokud hodnota předaná do `CreateRole` je metoda `Nothing` nebo je prázdný řetězec, výjimka vyvolána. Podobně pokud název role obsahuje čárkou je vyvolána výjimka. V důsledku toho stránka by měla obsahovat ovládací prvky pro ověřování zajistit, že uživatel zadá roli a zda neobsahuje žádné čárkami. Nechat jako cvičení pro čtečku.


Umožňuje vytvořit roli s názvem Správci. Přejděte `ManageRoles.aspx` stránky prostřednictvím prohlížeče, zadejte do textového pole ve Správci (viz obrázek 3) a potom klikněte na tlačítko Vytvořit roli.


[![Umožňuje vytvořit roli správce](creating-and-managing-roles-vb/_static/image8.png)](creating-and-managing-roles-vb/_static/image7.png)

**Obrázek 3**: umožňuje vytvořit roli správce ([Kliknutím zobrazit obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image9.png))


Co se stane? Dojde k zpětné volání, ale neexistuje žádné vizuální upozornění, která ve skutečnosti role byla přidána do systému. Budeme aktualizovat tuto stránku v kroku 5 a zahrnují vizuální zpětnou vazbu. Prozatím se však můžete ověřit, že byla role vytvořena přechodem na `SecurityTutorials.mdf` databáze a zobrazení dat z `aspnet_Roles` tabulky. Jak ukazuje obrázek 4, `aspnet_Roles` tabulka obsahuje záznam pro roli správce právě přidali.


[![Aspnet_Roles tabulka má řádek pro správce](creating-and-managing-roles-vb/_static/image11.png)](creating-and-managing-roles-vb/_static/image10.png)

**Obrázek 4**: `aspnet_Roles` tabulka obsahuje řádek pro správce ([Kliknutím zobrazit obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Krok 5: Zobrazení role v systému

Pojďme posílení `ManageRoles.aspx` stránky zahrnout seznam aktuální rolí v systému. K tomu, přidání ovládacího prvku GridView na stránku a nastavit jeho `ID` vlastnost `RoleList`. V dalším kroku přidejte metodu do třídy kódu stránky s názvem `DisplayRolesInGrid` pomocí následujícího kódu:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample8.vb)]

`Roles` Třídy `GetAllRoles` metoda vrátí všechny role v systému jako pole řetězců. Toto pole řetězce je pak vázána GridView. Aby bylo možné navázat seznam rolí GridView při prvním načtení stránky, musíme volání `DisplayRolesInGrid` metoda ze stránky `Page_Load` obslužné rutiny události. Následující kód zavolá tato metoda při první návštěvě stránce, ale ne na následné postback.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample9.vb)]

S tímto kódem na místě najdete na stránce prostřednictvím prohlížeče. Jak je vidět na obrázku 5, měli byste vidět mřížka s jedním sloupcem položka s názvem bez přípony. Mřížka obsahuje řádek pro roli správce, kterou jsme přidali v kroku 4.


[![GridView zobrazí role do jednoho sloupce](creating-and-managing-roles-vb/_static/image14.png)](creating-and-managing-roles-vb/_static/image13.png)

**Obrázek 5**: GridView zobrazí role do jednoho sloupce ([Kliknutím zobrazit obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image15.png))


GridView zobrazí jedinou sloupec s názvem položky, protože rutina GridView `AutoGenerateColumns` je nastavena na hodnotu True (výchozí), což způsobí, že rutina GridView automaticky vytvořit sloupec pro každou vlastnost v jeho `DataSource`. Pole má jednu vlastnost, která reprezentuje elementy v poli, proto jeden sloupec v GridView.

Při zobrazení dat pomocí GridView, raději nechci explicitně definovat Moje sloupců, nikoli je implicitně generované GridView mají. Explicitně definováním sloupce je mnohem snazší formátování data, změna uspořádání sloupců a provádět další běžné úlohy. Proto budeme aktualizovat prvku GridView deklarativní tak, aby její sloupce nejsou explicitně definovány.

Začněte tím, že nastavení prvku GridView `AutoGenerateColumns` vlastnost na hodnotu False. Dále přidejte TemplateField do mřížky, nastavte jeho `HeaderText` vlastnost rolí a nakonfigurujte jeho `ItemTemplate` tak, aby se zobrazí obsah pole. K tomu, přidání ovládacího prvku popisek Web s názvem `RoleNameLabel` k `ItemTemplate` a vytvořte vazbu jeho `Text` vlastnosti `Container.DataItem.`

Tyto vlastnosti a `ItemTemplate`na obsah je možné nastavit deklarativně nebo prostřednictvím dialogové okno pole prvku GridView a upravit šablony rozhraní. K dosažení pole dialogové okno, klikněte na odkaz Upravit sloupce v prvku GridView inteligentních značek. V dalším kroku, zrušte zaškrtnutí políčka automaticky generovat pole nastavit `AutoGenerateColumns` vlastnost na hodnotu False a přidejte TemplateField do GridView, nastavení jeho `HeaderText` vlastnost k roli. Chcete-li definovat `ItemTemplate`na obsah, zvolte možnost Upravit šablony z prvku GridView inteligentních značek. Přetáhněte ovládací prvek popisek webové na `ItemTemplate`, nastavte jeho `ID` vlastnost `RoleNameLabel`a nakonfigurujte jeho vazby dat tak, aby jeho `Text` vlastnost je vázána na `Container.DataItem`.

Bez ohledu na to, jakou metodu použijete by měla vypadat deklarativní výsledného prvku GridView podobný následujícímu po dokončení.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample10.aspx)]

> [!NOTE]
> Obsah tohoto pole se zobrazí pomocí syntaxe datové vazby `<%# Container.DataItem %>`. Proč se používá tato syntaxe při zobrazení obsahu pole vázána na GridView důkladné popis je nad rámec tohoto kurzu. Další informace v této věci, najdete v části [skalární hodnota pole vazby na ovládací prvek webového Data](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


V současné době `RoleList` GridView je vázaný jenom na seznam rolí, při první návštěvě stránky. Je potřeba aktualizovat mřížky vždy, když se přidá novou roli. K tomu, aktualizovat `CreateRoleButton` tlačítka `Click` obslužné rutiny události, které se volá `DisplayRolesInGrid` metoda, pokud se vytvoří novou roli.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample11.vb)]

Teď, když uživatel přidá novou roli `RoleList` GridView zobrazuje roli právě přidali na zpětné volání, poskytuje vizuální zpětnou vazbu, aby byla úspěšně vytvořená role. Pro znázornění je najdete `ManageRoles.aspx` stránky prostřednictvím prohlížeče a přidejte roli s názvem dohledu. Po kliknutí na tlačítko Vytvořit roli výsledkem by měla být zpětné volání a mřížky se aktualizuje a zahrnují správce a také novou roli dohledu.


[![Roli dohledu má byla přidána](creating-and-managing-roles-vb/_static/image17.png)](creating-and-managing-roles-vb/_static/image16.png)

**Obrázek 6**: vedoucí Role má přidané ([Kliknutím zobrazit obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Krok 6: Odstranění rolí

V tomto okamžiku může uživatel vytvořit novou roli a zobrazit všechny existující role z `ManageRoles.aspx` stránky. Umožňuje povolit uživatelům taky odstranění rolí. `Roles.DeleteRole` Metoda má dva přetížení:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Odstraní roli *roleName*. Pokud role obsahuje jeden nebo více členů, je vyvolána výjimka.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Odstraní roli *roleName*. Pokud *throwOnPopulateRole* je `True`, pak je vyvolána výjimka, pokud role obsahuje jeden nebo více členů. Pokud *throwOnPopulateRole* je `False`, role je odstraněn, zda obsahuje žádné členy, nebo ne. Interně `DeleteRole(roleName)` volání metod `DeleteRole(roleName, True)`.

`DeleteRole` Metoda také vyvolá výjimku, pokud se *roleName* je `Nothing` nebo prázdný řetězec nebo, pokud *roleName* obsahuje čárkami. Pokud *roleName* neexistuje v systému, `DeleteRole` selže tiše, bez vyvolání k výjimce.

Pojďme posílení GridView v `ManageRoles.aspx` zahrnout odstranění tlačítko, která po kliknutí na odstraní vybranou roli. Začněte přidáním tlačítko Odstranit GridView přejdete na dialogové okno pole a tlačítko odstranit, který je umístěný v části Možnosti CommandField přidáním. Ujistěte se, odstraňte tlačítko daleko levém sloupci a nastavit jeho `DeleteText` vlastnost odstranit roli.


[![Přidání tlačítka Odstranit do RoleList GridView](creating-and-managing-roles-vb/_static/image20.png)](creating-and-managing-roles-vb/_static/image19.png)

**Obrázek 7**: Přidání tlačítka Odstranit k `RoleList` GridView ([Kliknutím zobrazit obrázek v plné velikosti](creating-and-managing-roles-vb/_static/image21.png))


Po přidání tlačítko odstranit, vaše GridView deklarativní by měl vypadat takto:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample12.aspx)]

Dále vytvořte obslužnou rutinu události pro prvku GridView `RowDeleting` událostí. Toto je událost, která se vyvolá při zpětné volání při kliknutí na tlačítko odstranit roli. Přidejte následující kód do obslužné rutiny události.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample13.vb)]

Spustí kód prostřednictvím kódu programu odkazem `RoleNameLabel` webové ovládací prvek v řádku bylo stisknuto tlačítko jejichž odstranit roli. `Roles.DeleteRole` Potom je volána metoda, předávání v `Text` z `RoleNameLabel` a `False`, a tím odstranění role bez ohledu na to, jestli jsou všechny uživatele přidruženého k dané roli. Nakonec `RoleList` GridView se aktualizují, aby role právě odstraněné již nadále nebude zobrazovat v mřížce.

> [!NOTE]
> Tlačítko odstranit roli nevyžaduje žádné řazení potvrzení od uživatele před odstraněním role. Jedním z nejjednodušších způsobů, jak akci potvrďte je pomocí dialogového okna potvrdit na straně klienta. Další informace o této technice najdete v tématu [přidání Client-Side potvrzení při odstranění](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


## <a name="summary"></a>Souhrn

Mnoho webových aplikací mít určité autorizační pravidla nebo úrovně stránky funkce, které je k dispozici pro určité třídy uživatelů. Například může být sada webových stránek, kterým mají přístup pouze správci. Místo definování těchto autorizační pravidla na základě uživatele podle uživatelů, je často užitečnější umožňují definovat pravidla na základě role. Místo explicitně umožnit uživatelům Scott a Jisun přístup k webové stránky pro správu, více udržovatelný přístup je tak, aby povolovala členy rolí správce pro přístup k tyto stránek a potom k označení Scott a Jisun jako uživatele, kteří patří do Role správce.

Rozhraní framework rolí umožňuje snadno vytvořit a spravovat role. V tomto kurzu jsme se zaměřili na tom, jak nakonfigurovat rozhraní role použít `SqlRoleProvider`, který používá databázi systému Microsoft SQL Server jako úložiště rolí. Jsme vytvořili také na webové stránce, který uvádí existující role v systému a umožňuje vytvořit nové role a existující k odstranění. V následujících kurzech jsme se zobrazí, jak přiřadit uživatele k rolím a jak se má použít ověřování založené na rolích.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Zkoumání ASP.NET 2.0 je členství, role a profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Postupy: Použití správce rolí technologie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Zprostředkovateli rolí](https://msdn.microsoft.com/library/aa478950.aspx)
- [Vrácení vlastní nástroj pro správu webu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Technická dokumentace k `<roleManager>` – Element](https://msdn.microsoft.com/library/ms164660.aspx)
- [Pomocí členství a Role Správce rozhraní API](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu zahrnují Alicja Maziarz, Suchi Banerjee a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](role-based-authorization-cs.md)
> [další](assigning-roles-to-users-vb.md)
