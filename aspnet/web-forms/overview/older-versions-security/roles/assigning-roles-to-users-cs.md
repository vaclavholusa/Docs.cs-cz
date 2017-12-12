---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: "Přiřazení rolí pro uživatele (C#) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu vytvoříme dvě stránek ASP.NET, které pomáhají se správou, kterými se uživatelé patří do jaké rolí. Zařízení, které chcete zobrazit, co bude obsahovat na první stránku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: 752882b16fe80cc99c9f333bcc2067e677e6670b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="assigning-roles-to-users-c"></a>Přiřazení rolí pro uživatele (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> V tomto kurzu vytvoříme dvě stránek ASP.NET, které pomáhají se správou, kterými se uživatelé patří do jaké rolí. První stránka bude obsahovat zařízení, které chcete zobrazit, co uživatelé patří do dané role, jaké role konkrétní uživatel patří do a možnost přiřazení nebo odebrání určitého uživatele z konkrétní roli. Na druhé stránce jsme posílení ovládacího prvku CreateUserWizard tak, že obsahují krok určit, jaké role nově vytvořeného uživatele patří do. To je užitečné v situacích, kde je možné vytvořit nové uživatelské účty správce.


## <a name="introduction"></a>Úvod

<a id="_msoanchor_1"> </a> [Předchozí kurzu](creating-and-managing-roles-cs.md) zkontrolován rozhraní role a `SqlRoleProvider`; jsme viděli, jak používat `Roles` třídy pro vytvoření, získání a odstranění rolí. Kromě vytvoření nebo odstranění rolí, potřebujeme mít možnost přiřazení nebo odebrání uživatele z role. ASP.NET bohužel se nedodává s všech webových ovládacích prvků pro správu, kterými se uživatelé patří do jaké rolí. Místo toho jsme musíte vytvořit vlastní stránky ASP.NET ke správě těchto přidružení. Dobrá zpráva je, že přidání a odebrání uživatele k rolím je poměrně snadné. `Roles` Třída obsahuje několik metod pro přidání uživatelů do jedné nebo více rolí.

V tomto kurzu vytvoříme dvě stránek ASP.NET, které pomáhají se správou, kterými se uživatelé patří do jaké rolí. První stránka bude obsahovat zařízení, které chcete zobrazit, co uživatelé patří do dané role, jaké role konkrétní uživatel patří do a možnost přiřazení nebo odebrání určitého uživatele z konkrétní roli. Na druhé stránce jsme posílení ovládacího prvku CreateUserWizard tak, že obsahují krok určit, jaké role nově vytvořeného uživatele patří do. To je užitečné v situacích, kde je možné vytvořit nové uživatelské účty správce.

Můžeme začít!

## <a name="listing-what-users-belong-to-what-roles"></a>Výpis kterými se uživatelé patří do jaké rolí

První obchodní of pořadí v tomto kurzu je vytvoření webové stránky, ze kterého lze přiřadit uživatele k rolím. Před jsme týkají označována s přiřazení uživatele k rolím, můžeme nejprve soustředit se jenom na tom, jak zjistit, co uživatelé patří do jaké role. Existují dva způsoby, jak zobrazit tyto informace: "role" nebo "uživatelem." Jsme by se mohl musí návštěvníka vyberte roli a potom je zobrazit všechny uživatele, které patří k roli ("podle role" displeji) nebo jsme může vyzvat návštěvníka vyberte uživatele a pak je zobrazit role přiřazené pro tohoto uživatele ("uživatelem" displeji).

Zobrazení "podle role" je užitečné v případech kde návštěvníka chce vědět sadu uživatelů, které patří do konkrétní roli; zobrazení "uživatelem" je ideální pro návštěvníka musí znát rolí konkrétní uživatele. Umožňuje mít naší stránce s zahrnují "podle role" i "uživatelem" rozhraní.

Nemůžeme se spustí s vytvářením rozhraní "uživatelem". Toto rozhraní se skládá z rozevíracího seznamu a seznam zaškrtávacích políček. Rozevíracím seznamu bude možné naplnit sadu uživatelů systému v systému. Zaškrtávací políčka se zobrazí seznam rolí. Výběr uživatele z rozevíracího seznamu zkontroluje tyto role, které uživatel patří. Osoba, která na stránce můžete pak zaškrtněte nebo zrušte zaškrtnutí políček můžete přidat nebo odebrat vybrané uživatele z odpovídající role.

> [!NOTE]
> Pomocí rozevíracího seznamu seznamu uživatelských účtů není ideálním řešením pro weby tam, kde může být stovky uživatelské účty. Rozevíracím seznamu je navržena k umožnění uživatelům vybrat jednu položku ze seznamu poměrně krátké možností. Rychlé naroste nepraktické s růstem počet položek seznamu. Pokud vytváříte web, který bude mít potenciálně velkého počtu uživatelských účtů, můžete chtít zvažte použití alternativní uživatelské rozhraní, například stránkovatelné GridView nebo filtrování rozhraní, které jsou uvedené vyzve návštěvníka zvolit písmeno a potom pouze Zobrazuje uživatele, jejichž uživatelské jméno při spuštění vybraného písmeno.


## <a name="step-1-building-the-by-user-user-interface"></a>Krok 1: Vytvoření uživatelského rozhraní "Uživatelem"

Otevřete `UsersAndRoles.aspx` stránky. V horní části stránky, přidání ovládacího prvku popisek Web s názvem `ActionStatus` a vymažte její `Text` vlastnost. Tento popisek použijeme k poskytnutí zpětné vazby akce provést, při zobrazení zprávy, jako je, "Tito uživatele byla přidána do role Správci" nebo "Jisun uživatele byl odebrán z role vedoucí." Aby bylo možné tyto zprávy zvýrazní, nastavte jeho `CssClass` vlastnost "Důležité".

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Dál přidejte následující definici třídy CSS na `Styles.css` šablony stylů:

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Tuto definici šablon stylů CSS dostane pokyn, aby zobrazení pomocí velký, červené písmo popisku. Obrázek 1 zobrazuje tento efekt prostřednictvím návrháři sady Visual Studio.


[![Vlastnost CssClass jmenovky výsledkem velký, Red písma](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Obrázek 1**: popisku `CssClass` vlastnost má za následek velké, písmo červený ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image3.png))


Dále přidejte rozevírací seznam na stránku, nastavte jeho `ID` vlastnost `UserList`a nastavit jeho `AutoPostBack` vlastnost na hodnotu True. Seznam všech uživatelů v systému použijeme této rozevírací seznam. Tento rozevírací seznam bude vázán na kolekci objektů MembershipUser. Vzhledem k tomu, že chceme rozevírací seznam zobrazí uživatelské jméno vlastnost objektu MembershipUser (a použít jako hodnotu položky seznamu), nastavte rozevírací seznam `DataTextField` a `DataValueField` vlastnosti "Uživatelské jméno".

Pod rozevírací seznam, přidejte opakovače s názvem `UsersRoleList`. Tato opakovače zobrazí seznam všech rolí v systému jako řadu zaškrtávací políčka. Definování opakovače `ItemTemplate` pomocí následující kód:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

`ItemTemplate` Značek obsahuje jeden ovládací prvek zaškrtávací políčko Web s názvem `RoleCheckBox`. Na zaškrtávací políčko `AutoPostBack` je nastavena na hodnotu True a `Text` vlastnost je vázána na `Container.DataItem`. Z důvodu syntaxe vazby dat je jednoduše `Container.DataItem` je, že rozhraní role vrátí seznam názvů rolí jako pole řetězců a je toto pole řetězec, který jsme bude vytvoření vazby na opakovače. Proč této syntaxe slouží k zobrazení obsahu pole svázán s ovládacím Web data důkladné popis je nad rámec tohoto kurzu. Další informace v této věci, najdete v části [skalární hodnota pole vazby na ovládací prvek webového Data](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

V tomto okamžiku deklarativní vaší "uživatelem" rozhraní by měl vypadat takto:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

Teď připravení psát kód pro vazbu sadu uživatelské účty rozevírací seznam a sadu rolí pro opakovače. Do třídy kódu stránky, přidejte metodu s názvem `BindUsersToUserList` a druhou s názvem `BindRolesList`, pomocí následujícího kódu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

`BindUsersToUserList` Metoda načte všechny uživatelské účty v systému prostřednictvím [ `Membership.GetAllUsers` metoda](https://msdn.microsoft.com/en-us/library/dy8swhya.aspx). Tento příkaz vrátí [ `MembershipUserCollection` objekt](https://msdn.microsoft.com/en-us/library/system.web.security.membershipusercollection.aspx), což je kolekce [ `MembershipUser` instance](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx). Tato kolekce je pak vázána `UserList` rozevírací seznam. `MembershipUser` Instance tento způsob vytvoření kolekce obsahovat celou řadu vlastností, jako je `UserName`, `Email`, `CreationDate`, a `IsOnline`. Chcete-li pokyn rozevírací seznam zobrazíte hodnotu `UserName` vlastnost, ujistěte se, že `UserList` na rozevírací seznam `DataTextField` a `DataValueField` vlastnosti byly nastaveny jako "UserName".

> [!NOTE]
> `Membership.GetAllUsers` Metoda má dva přetížení: jeden, který přijímá žádné vstupní parametry a vrátí všechny uživatele a ten, který přebírá celočíselné hodnoty pro index stránky a velikost stránky a vrátí pouze zadané podmnožině uživatelů. Po velkých objemů uživatelské účty, které se zobrazuje v element stránkovatelné uživatelského rozhraní se druhý přetížení umožňuje efektivněji stránky do uživatele od vrátí jen přesné dílčí uživatelské účty a ne všechny z nich.


`BindRolesToList` Metoda spustí voláním `Roles` třídy [ `GetAllRoles` metoda](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getallroles.aspx), která vrací pole řetězců obsahující role v systému. Toto pole řetězce je pak vázána opakovače.

Nakonec je potřeba volat tyto dvě metody při prvním načtení stránky. Přidejte následující kód, který `Page_Load` obslužné rutiny události:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

S tímto kódem na místě pozorně navštívit stránku prostřednictvím prohlížeče; na obrazovce by měla vypadat podobně jako na obrázku 2. Všechny uživatelské účty, naplní se v rozevíracím seznamu a pod, každou roli, zobrazí se jako zaškrtávací políčko. Protože jsme nastavena `AutoPostBack` vlastnosti rozevírací seznam a zaškrtávací políčka na hodnotu True, změna vybraného uživatele nebo kontrola nebo zrušte zaškrtnutí role způsobí, že zpětné volání. Neprovede se žádná akce, ale protože jsme ještě chcete napsat kód pro zpracování těchto akcí. Tyto úlohy v následujících dvou částech jsme budete řešení.


[![Stránka zobrazí uživatelů a rolí](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Obrázek 2**: stránka zobrazí uživatelů a rolí ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Kontrola role vybraného uživatele patří do

Při prvním načtení stránky, nebo vždy, když návštěvníka vybere nového uživatele z rozevíracího seznamu, je potřeba aktualizovat `UsersRoleList`na zaškrtnutí jednotlivých políček tak, aby daný roli zaškrtávací políčko je zaškrtnuté pouze v případě, že vybraný uživatel patří do této role. K tomu, vytvořte metodu s názvem `CheckRolesForSelectedUser` následujícím kódem:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

Výše uvedený kód spustí tak, že určíte, který je vybraný uživatel. Poté používá třídu role [ `GetRolesForUser(userName)` metoda](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getrolesforuser.aspx) vrátit zadaný uživatel sadu role jako pole řetězců. Dále jsou uvedené položky Opakovači a každá položka `RoleCheckBox` zaškrtávací políčko se odkazuje prostřednictvím kódu programu. Zaškrtávací políčko je zaškrtnuté, jenom v případě, že je součástí role odpovídá `selectedUsersRoles` pole řetězců.

> [!NOTE]
> `selectedUserRoles.Contains<string>(...)` Syntaxe nebude kompilovat, pokud používáte technologii ASP.NET verze 2.0. `Contains<string>` Metoda je součástí [LINQ knihovny](http://en.wikipedia.org/wiki/Language_Integrated_Query), což je nová technologie ASP.NET 3.5. Pokud stále používáte technologii ASP.NET verze 2.0, použijte [ `Array.IndexOf<string>` metoda](https://msdn.microsoft.com/en-us/library/eha9t187.aspx) místo.


`CheckRolesForSelectedUser` Metoda musí být volána ve dvou případech: při prvním načtení stránky a vždy, když `UserList` vybraného indexu na rozevírací seznam se změnilo. Proto volat tuto metodu z `Page_Load` obslužné rutiny události (po volání `BindUsersToUserList` a `BindRolesToList`). Navíc vytvoření obslužné rutiny události pro rozevírací seznam `SelectedIndexChanged` událostí a volání této metody odtud.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

S tímto kódem na místě můžete testovat stránky prostřednictvím prohlížeče. Ale protože `UsersAndRoles.aspx` stránky aktuálně nemá schopnost přiřadit uživatele k rolím, uživatelé nemají role. Vytvoříme rozhraní pro přiřazování uživatelů do rolí ve chvíli, abyste mohli provést Moje slovo, které lze použít tento kód a ověřte, že dělá to tak později, nebo můžete ručně přidat uživatele k rolím vložením záznamů do `aspnet_UsersInRoles` tabulky účely otestování tohoto functi Nyní onality.

### <a name="assigning-and-removing-users-from-roles"></a>Přiřazení a odebrat uživatele z role

Když návštěvníka kontroluje nebo zruší tohoto systému zaškrtnutí zaškrtávacího políčka v `UsersRoleList` opakovače je potřeba přidat nebo odebrat vybrané uživatele z odpovídající role. Na zaškrtávací políčko `AutoPostBack` je aktuálně nastavena na hodnotu True, což způsobí, že zpětné volání kdykoliv v Opakovači zaškrtávací políčko je zaškrtnuté nebo nezaškrtnuté. Stručně řečeno, je potřeba vytvořit obslužnou rutinu události pro na zaškrtávací políčko `CheckChanged` událostí. Vzhledem k tomu, že je zaškrtávací políčko v ovládacím prvku opakovače, je potřeba ručně přidat vložení obslužné rutiny událostí. Začněte přidáním obslužné rutiny události pro třídu kódu jako `protected` metoda, například takto:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Vrátí jsme napsat kód pro této obslužné rutiny události za chvíli. První, ale umožňuje dokončení zpracování vložení událostí. Z políčka v rámci opakovače `ItemTemplate`, přidejte `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Vedení vodiče této syntaxe `RoleCheckBox_CheckChanged` obslužná rutina události `RoleCheckBox`na `CheckedChanged` událostí.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

Naše poslední, je k dokončení `RoleCheckBox_CheckChanged` obslužné rutiny události. Je potřeba spustit tak, že odkazující na ovládací prvek zaškrtávací políčko, který vyvolá událost, protože tato instance políčko víme, jakou roli bylo zaškrtnuté nebo nezaškrtnuté prostřednictvím jeho `Text` a `Checked` vlastnosti. Na základě těchto informací společně s uživatelské jméno vybraného uživatele nemůžeme přidat nebo odebrat uživatele z role prostřednictvím `Roles` třídy [ `AddUserToRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.addusertorole.aspx) nebo [ `RemoveUserFromRole` metoda](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

Výše uvedený kód spustí prostřednictvím kódu programu odkazem na zaškrtávací políčko, který vyvolá událost, která je k dispozici prostřednictvím `sender` vstupní parametr. Pokud je zaškrtnuté políčko, vybraný uživatel přidán do zadané roli, jinak budou odstraněny z role. V obou případech `ActionStatus` popisek zobrazí zprávu shrnutí stačí provést akci.

Za chvíli k otestování této stránce prostřednictvím prohlížeče. Vyberte uživatele Tito a poté přidejte Tito správci i vedoucí rolí.


[![Tito přidala pro správce a vedoucí role](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Obrázek 3**: Tito přidala pro správce a role vedoucí ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image9.png))


V dalším kroku vyberte uživatele Bruce z rozevíracího seznamu. Je zpětné volání a zaškrtávací políčka opakovače jsou aktualizovány pomocí `CheckRolesForSelectedUser`. Vzhledem k tomu, že Bruce zatím nepatří do žádné role, jsou zaškrtnuté políčko dvou políček. Dál přidejte Bruce k roli dohledu.


[![Bruce se přidal k roli dohledu](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Obrázek 4**: Bruce se přidal k roli dohledu ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image12.png))


Provést ještě další ověření funkce `CheckRolesForSelectedUser` metoda, vyberte uživatele, než Tito nebo Bruce. Všimněte si, jak jsou automaticky není zaškrtnuto zaškrtávací políčka, které označuje, že jejich nepatří k žádné roli. Vraťte se do Tito. Zkontrolovat správci i vedoucí zaškrtávací políčka.

## <a name="step-2-building-the-by-roles-user-interface"></a>Krok 2: Vytvoření uživatelského rozhraní "Rolemi"

V tomto okamžiku jste dokončili rozhraní "uživatelem" a budete připravení zahájit boji se rozhraní "rolemi". Rozhraní "rolemi" vyzývá uživatele k roli vyberte z rozevíracího seznamu a potom zobrazí sadu uživatelů, kteří patří do této role v GridView.

Přidejte další ovládací prvek rozevírací seznam k `UsersAndRoles.aspx` stránky. Platnost této jednu ovládacím prvku Repeater, a pojmenujte ji `RoleList`a nastavit jeho `AutoPostBack` vlastnost na hodnotu True. Pod, přidejte GridView a pojmenujte ji `RolesUserList`. Tato rutina GridView zobrazí seznam uživatelů, kteří patří do vybrané role. Nastavte GridView `AutoGenerateColumns` vlastnost na hodnotu False, přidejte do mřížky TemplateField `Columns` kolekce a nastavte její `HeaderText` vlastnost "Uživatelům". Definování TemplateField `ItemTemplate` tak, aby se zobrazí hodnota výrazu datové vazby `Container.DataItem` v `Text` vlastnost štítek s názvem `UserNameLabel`.

Po přidání a nakonfigurování GridView, deklarativní vaší "podle role" rozhraní by měl vypadat takto:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

Potřebujeme k naplnění `RoleList` rozevírací seznam s sadu rolí v systému. Pokud chcete dosáhnout, aktualizovat `BindRolesToList` metoda tak, aby se vazby vrátil pole řetězce `Roles.GetAllRoles` metodu `RolesList` rozevírací seznam (a taky `UsersRoleList` opakovače).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

Poslední dva řádky v `BindRolesToList` metoda byly přidány do sadu rolí pro vytvoření vazby `RoleList` ovládací prvek rozevírací seznam. Obrázek 5 ukazuje konečný výsledek při zobrazení prostřednictvím prohlížeče – rozevíracího seznamu naplněný role systému.


[![Role jsou zobrazeny v rozevírací seznam RoleList](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Obrázek 5**: role se zobrazují v `RoleList` rozevírací seznam ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Zobrazení uživatelů, kteří patří k vybrané roli

Při prvním načtení stránky, nebo když je vybrána novou roli z `RoleList` rozevírací seznam, musíme zobrazte seznam uživatelů, kteří patří do této role v GridView. Vytvořit metodu s názvem `DisplayUsersBelongingToRole` pomocí následujícího kódu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Tato metoda začíná získáním vybranou roli z `RoleList` rozevírací seznam. Poté použije [ `Roles.GetUsersInRole(roleName)` metoda](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getusersinrole.aspx) k načtení pole řetězců uživatelských jmen uživatelů, které patří do této role. Toto pole je pak vázána `RolesUserList` GridView.

Tato metoda musí být volána dvěma způsoby: při počátečním načtení stránky a při vybranou roli v `RoleList` změny rozevírací seznam. Proto aktualizovat `Page_Load` obslužné rutiny události tak, aby tato metoda je volána po volání `CheckRolesForSelectedUser`. Dále vytvořte obslužnou rutinu události pro `RoleList`na `SelectedIndexChanged` událostí a příliš volat tuto metodu odtud.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

S tímto kódem na místě `RolesUserList` GridView má zobrazit uživatelům, kteří patří do vybrané role. Jak je vidět na obrázku 6, roli dohledu se skládá z dva členy: Bruce a Tito.


[![GridView uvádí uživatelům, kteří patří k vybrané roli](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Obrázek 6**: The GridView uvádí těch, které uživatelé, patří do vybrané Role ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Odebrat uživatele z vybrané Role.

Pojďme posílení `RolesUserList` GridView tak, že obsahují sloupec "Odebrat" tlačítka. Klepnutím na tlačítko "Odebrat" pro konkrétního uživatele, budou odebrány z této role.

Začněte přidáním pole tlačítko Odstranit pro GridView. Ujistěte se, v tomto poli se zobrazí jako nejvíce archivované doleva a změňte jeho `DeleteText` vlastnost z "Odstranit" (výchozí) na "Odebrat".


[![Přidat](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Obrázek 7**: Přidání tlačítka "Odebrat" GridView ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image21.png))


Při kliknutí na tlačítko "Odebrat" vyplývá zpětné volání a prvku GridView `RowDeleting` událost se vyvolá. Je potřeba vytvořit obslužnou rutinu události pro tuto událost a napsat kód, který odebere uživatele z vybrané role. Vytvoření obslužné rutiny události a poté přidejte následující kód:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

Spustí kód tak, že určíte název vybrané role. Následně prostřednictvím kódu programu odkazy `UserNameLabel` řízení z řádku, jehož tlačítko "Odebrat" označeného aby mohla určit uživatelské jméno uživatele k odebrání. Uživatele se pak odebral z role prostřednictvím volání `Roles.RemoveUserFromRole` metoda. `RolesUserList` Neobnovíte GridView a zobrazí se zpráva prostřednictvím `ActionStatus` popisku ovládacího prvku.

> [!NOTE]
> Tlačítko "Odebrat" nevyžaduje žádné řazení potvrzení od uživatele před odebráním uživatele z role. Mohu pozvat můžete přidat určitou úrovní potvrzení uživatele. Jedním z nejjednodušších způsobů, jak akci potvrďte je pomocí dialogového okna potvrdit na straně klienta. Další informace o této technice najdete v tématu [přidání Client-Side potvrzení při odstranění](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Obrázek 8 ukazuje na stránku uživatele Tito bude po odebrání ze skupiny dohledu.


[![Bohužel Tito již není Supervisor](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Obrázek 8**: bohužel, Tito již není Supervisor ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Přidání nových uživatelů do vybrané Role.

Společně se odebrat uživatele z vybrané role, návštěvníka na této stránce taky by mohli přidat uživatele do vybrané role. Nejlepší rozhraní pro přidání uživatele do vybrané role závisí na počtu uživatelských účtů, které chcete mít. Pokud váš web bude obsahovat několika desítek uživatelské účty nebo méně, můžete použít rozevírací seznam v tomto poli. Pokud může být tisíce uživatelské účty, by chcete zahrnout uživatelské rozhraní, která umožňuje návštěvníka stránky prostřednictvím účtů, hledání konkrétního účtu, nebo filtrování uživatelské účty nějakým způsobem.

Pro tuto stránku použijeme velmi jednoduché rozhraní, které funguje bez ohledu na počet uživatelských účtů v systému. Konkrétně použijeme textovému poli, zobrazení výzvy zadejte uživatelské jméno uživatele, kterého chce přidat do vybrané role musí návštěvníka. Pokud neexistuje žádný uživatel s tímto názvem, nebo pokud uživatel je již členem role, jsme vám zobrazí zpráva v `ActionStatus` popisek. Ale pokud uživatel existuje a není členem role, jsme budete je přidejte do role a aktualizujte mřížky.

Přidáte textové pole a tlačítko pod GridView. Nastavit textového pole `ID` k `UserNameToAddToRole` a nastavte na tlačítko `ID` a `Text` vlastnosti, které chcete `AddUserToRoleButton` a "Přidat uživatele k roli", v uvedeném pořadí.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

Dále vytvořte `Click` obslužné rutiny události pro `AddUserToRoleButton` a přidejte následující kód:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

Většina kód `Click` obslužné rutiny události provádí různé ověřovací kontroly. Zajišťuje, že návštěvníka zadané uživatelské jméno v `UserNameToAddToRole` textovému poli, zda uživatel existuje v systému, a zda již nepatří do vybrané role. Pokud některý z těchto kontrol selže, zobrazí se příslušná zpráva v `ActionStatus` a obslužné rutiny události je byl ukončen. Pokud všechny kontroly úspěšně, uživatel je přidán k roli prostřednictvím `Roles.AddUserToRole` metoda. Následující tedy textové pole na `Text` je vlastnost bylo vymazáno, GridView aktualizaci a `ActionStatus` popisek zobrazí zprávu s upozorněním, že zadaný uživatel byl úspěšně přidán do vybrané role.

> [!NOTE]
> Aby se zajistilo, že zadaný uživatel již nepatří do vybrané role, používáme [ `Roles.IsUserInRole(userName, roleName)` metoda](https://msdn.microsoft.com/en-us/library/system.web.security.roles.isuserinrole.aspx), která vrací logickou hodnotu, která určuje zda *uživatelské jméno* je členem *roleName*. Budeme používat tuto metodu znovu v <a id="_msoanchor_2"> </a> [další kurz](role-based-authorization-cs.md) při se podíváme na ověření na základě role.


Navštívit stránku prostřednictvím prohlížeče a vyberte roli dohledu z `RoleList` rozevírací seznam. Zkuste zadat neplatného uživatelského jména – se měla zobrazit zpráva, která vysvětluje, že uživatel neexistuje v systému.


[![Nelze přidat neexistující uživatele k roli](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Obrázek 9**: nelze přidat neexistující bez uživatele k roli ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image27.png))


Nyní zkuste přidat platného uživatele. Pokračovat a znovu přidat Tito k roli dohledu.


[![Tito je ještě jednou Supervisor!](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Obrázek 10**: Tito je ještě jednou Supervisor!  ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Krok 3: Cross aktualizace "Uživatelem" a "Role" rozhraní

`UsersAndRoles.aspx` Stránka nabízí dvě odlišné rozhraní pro správu uživatelů a rolí. V současné době tato dvě rozhraní fungují nezávisle na ostatních, takže je možné, že změny provedené v jedno rozhraní se neprojeví okamžitě v dalších. Představte si například, že návštěvníka na stránku vybere roli dohledu z `RoleList` rozevírací seznam, který obsahuje seznam Bruce a Tito jako členy. V dalším kroku návštěvníka vybere Tito z `UserList` rozevírací seznam, který kontroluje správci a vedoucí políček v `UsersRoleList` opakovače. Pokud návštěvníka pak zruší tohoto systému zaškrtnutí roli nadřízeného z opakovače, Tito je odebrán z role. vedoucí, ale tato úprava se nereflektují v rozhraní "podle role". GridView, bude mít Tito jako člena role orgánů dohledu.

Chcete-li tomu je potřeba aktualizovat GridView vždy, když je role zaškrtnuté nebo nezaškrtnuté z `UsersRoleList` opakovače. Podobně musíme aktualizovat opakovače vždy, když uživatel se přidaly nebo odebraly roli z rozhraní "podle role".

Opakovače v rozhraní "uživatelem" obnovován při volání `CheckRolesForSelectedUser` metoda. Rozhraní "podle role" lze upravit `RolesUserList` GridView `RowDeleting` obslužné rutiny události a `AddUserToRoleButton` tlačítka `Click` obslužné rutiny události. Proto je potřeba volat `CheckRolesForSelectedUser` metoda z každé z těchto metod.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

Podobně se aktualizují GridView v rozhraní "podle role" voláním `DisplayUsersBelongingToRole` "uživatelem" rozhraní a metoda je upraveny prostřednictvím `RoleCheckBox_CheckChanged` obslužné rutiny události. Proto je potřeba volat `DisplayUsersBelongingToRole` metoda z této obslužné rutiny události.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Tyto změny menší kódu "uživatelem" a "role" rozhraní nyní správně cross-update. Chcete-li to ověřit, najdete na stránce prostřednictvím prohlížeče a vyberte Tito a vedoucí z `UserList` a `RoleList` DropDownLists, v uvedeném pořadí. Všimněte si, že jako zrušte zaškrtnutí políčka roli vedoucí k Tito z opakovače v rozhraní "uživatelem" Tito automaticky odebrána z GridView v rozhraní "podle role". Přidání Tito zpět k roli dohledu z rozhraní "podle role" automaticky znovu ověří políčka vedoucí v rozhraní "uživatelem".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Krok 4: Přizpůsobení CreateUserWizard zahrnout krok "Zadejte role"

V <a id="_msoanchor_3"> </a> [ *vytváření uživatelských účtů* ](../membership/creating-user-accounts-cs.md) kurzu jsme viděli, jak používat ovládací prvek webu CreateUserWizard k poskytnutí rozhraní pro vytvoření nového uživatelského účtu. Ovládací prvek CreateUserWizard lze v jednom ze dvou způsobů:

- Jako prostředek pro návštěvníky vytvořit vlastní uživatelský účet na webu a
- Jako prostředek pro vytvoření nových účtů správce

V prvním případě použijte návštěvníka na web a vyplní CreateUserWizard, zadáte své informace pro registraci na webu. V druhém případě správce vytvoří nový účet jiné osoby.

Když probíhá vytváření účtu správce pro jinou osobu, může být užitečné umožňují správcům určit, jaké role nový uživatelský účet patří do. V <a id="_msoanchor_4"> </a> [ *ukládání* *Další informace o uživateli* ](../membership/storing-additional-user-information-cs.md) kurzu jsme viděli, jak přizpůsobit: můžete přidat další CreateUserWizard `WizardSteps`. Podíváme, jak přidat další krok CreateUserWizard a zadejte nové uživatelské role.

Otevřete `CreateUserWizardWithRoles.aspx` stránky a přidání ovládacího prvku CreateUserWizard s názvem `RegisterUserWithRoles`. Nastavte ovládacího prvku `ContinueDestinationPageUrl` vlastnost "~ / Default.aspx". Protože Rada tady je, že správce používat tento ovládací prvek CreateUserWizard k vytvoření nových uživatelských účtů, nastavte ovládacího prvku `LoginCreatedUser` vlastnost na hodnotu False. To `LoginCreatedUser` vlastnost určuje, jestli návštěvníka je automaticky přihlášeni jako uživatel právě vytvořili, a je standardně nastavena na hodnotu True. Nemůžeme nastavit na hodnotu False, protože když správce vytvoří nový účet chceme, aby mu přihlášení jako sám.

Potom vyberte "Přidat nebo odebrat `WizardSteps`..." možnost z inteligentních značek CreateUserWizard a přidejte nový `WizardStep`, nastavení jeho `ID` k `SpecifyRolesStep`. Přesunout `SpecifyRolesStep WizardStep` tak, aby pochází za krok "Sign k si nový účet", ale před "Úplná" krok. Nastavte `WizardStep`na `Title` vlastnost "Zadejte role", jeho `StepType` vlastnost `Step`a jeho `AllowReturn` vlastnost na hodnotu False.


[![Přidat](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Obrázek 11**: přidání "Zadejte rolí" `WizardStep` k CreateUserWizard ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image33.png))


Po této změně vaší CreateUserWizard deklarativní by měl vypadat následovně:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

V části "Zadejte role" `WizardStep`, přidejte třídy CheckBoxList s názvem `RoleList`. Tato CheckBoxList zobrazí seznam dostupných rolí povolení uživatelům na stránce zkontrolujte, jaké role nově vytvořeného uživatele patří do.

Jsme je tedy dvě úlohy kódování: nejprve musí naplníme `RoleList` CheckBoxList s rolemi systému; druhý, je potřeba přidat vytvořený uživatel na vybrané role, když se uživatel přesune z kroku "Zadejte role" na "Dokončit" krok. První úlohou jsme můžete provést `Page_Load` obslužné rutiny události. Následující kód prostřednictvím kódu programu odkazy `RoleList` zaškrtávací políčko je na první přejděte na stránku a váže role v systému k němu.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

Ve výše uvedeném kódu by měla vypadat povědomě. V <a id="_msoanchor_5"> </a> [ *ukládání* *Další informace o uživateli* ](../membership/storing-additional-user-information-cs.md) kurzu jsme použili dva `FindControl` příkazy tak, aby odkazovaly ovládací prvek webu z v rámci vlastní `WizardStep`. A kód, který vytvoří vazbu role třídy CheckBoxList byl získán z dříve v tomto kurzu.

Aby bylo možné provést v druhé úloze programovací potřebujeme vědět při dokončení kroku "Zadejte role". Odvolání, který má CreateUserWizard `ActiveStepChanged` událost, která aktivuje se pokaždé, když návštěvníka přejde z jednoho kroku do jiné. Zde jsme můžete určit, pokud uživatel dosáhl "Dokončit" krok; Pokud ano, je potřeba přidat uživatele do vybrané role.

Vytvoření obslužné rutiny událostí `ActiveStepChanged` událostí a přidejte následující kód:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Pokud uživatel právě dosáhl "Dokončeno" krok, obslužné rutiny události zobrazí položky `RoleList` CheckBoxList a právě vytvořené uživatelem je přiřazen k vybrané role.

Navštivte tuto stránku prostřednictvím prohlížeče. Prvním krokem při CreateUserWizard je standardní krok "Sign k si nový účet", který zobrazí výzvu pro nové uživatelské jméno, heslo, e-mailu a další informace o klíči. Zadejte informace pro vytvoření nového uživatele s názvem Wanda.


[![Vytvořte nového uživatele s názvem Wanda](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Obrázek 12**: Vytvořte nový účet uživatele s názvem Wanda ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image36.png))


Klikněte na tlačítko "Vytvořit uživatele". CreateUserWizard interně volá `Membership.CreateUser` metody vytvoření nového uživatelského účtu a potom pokračuje k dalšímu kroku, "Zadejte role serveru." Tady jsou uvedené role systému. Zaškrtnutím políčka dohledu a klikněte na tlačítko Další.


[![Nastavte Wanda člena roli dohledu](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Obrázek 13**: nastavte Wanda člena roli dohledu ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image39.png))


Kliknutím na tlačítko Další způsobí, že zpětné volání a aktualizací `ActiveStep` na krok "Dokončeno". V `ActiveStepChanged` obslužné rutiny události, nedávno vytvořili uživatelský účet je přiřazen k roli dohledu. Chcete-li to ověřit, vraťte se k `UsersAndRoles.aspx` a vyberte vedoucí z `RoleList` rozevírací seznam. Jak ukazuje obrázek 14, vedoucí je nyní tvoří tři uživatelé: Bruce, Tito a Wanda.


[![Bruce, Tito a Wanda jsou všechny vedoucí](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Obrázek 14**: Bruce, Tito a Wanda jsou všechny vedoucí ([Kliknutím zobrazit obrázek v plné velikosti](assigning-roles-to-users-cs/_static/image42.png))


## <a name="summary"></a>Souhrn

Rozhraní framework rolí nabízí metody pro načtení informací o rolích a metody pro určení, které uživatelé patří do zadané roli určitého uživatele. Kromě toho existuje několik metod pro přidávání a odebírání jeden nebo více uživatelů k jedné nebo více rolí. V tomto kurzu jsme zaměřené na právě dva z těchto metod: `AddUserToRole` a `RemoveUserFromRole`. Existují další variant určená k přidání více uživatelů do jedné role a přiřadit víc rolí na jednoho uživatele.

V tomto kurzu zahrnuty také podívejte se na rozšíření ovládacího prvku CreateUserWizard zahrnout `WizardStep` můžete zadat role nově vytvořeného uživatele. Takové krok může pomoct správce zjednodušit proces vytváření uživatelských účtů pro nové uživatele.

V tuto chvíli jsme viděli jste, jak vytvářet a odstraňovat role a jak přidávat a odebírat uživatele z role. Ale ještě se podívat na použití ověřování na základě rolí. V <a id="_msoanchor_6"> </a> [následující kurzu](role-based-authorization-cs.md) Podíváme se na definování autorizačních pravidel adres URL na základě rolí role také jak omezit tak jeho funkčnost úrovně stránky na základě rolí aktuálně přihlášeného uživatele.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Přehled nástroje pro správu webu ASP.NET](https://msdn.microsoft.com/en-us/library/ms228053.aspx)
- [Zkoumání ASP. Členství, role a profil pro NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Vrácení vlastní nástroj pro správu webu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování...

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](creating-and-managing-roles-cs.md)
[další](role-based-authorization-cs.md)
