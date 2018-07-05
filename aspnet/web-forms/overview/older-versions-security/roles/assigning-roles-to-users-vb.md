---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Přiřazení rolí uživatelům (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu vytvoříme dvě stránky technologie ASP.NET pro pomoc se správou, které uživatelé patří do rolích. Zařízení, abyste zjistili, co bude obsahovat na první stránku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: e0c653a53e63fd533d4d5490a08249d33edc94fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397046"
---
<a name="assigning-roles-to-users-vb"></a>Přiřazení rolí uživatelům (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> V tomto kurzu vytvoříme dvě stránky technologie ASP.NET pro pomoc se správou, které uživatelé patří do rolích. První stránka bude obsahovat zařízení, pokud chcete zobrazit, co uživatelé patří do dané role, ke kterým rolím konkrétní uživatel patří do a možnost přiřazení nebo odebrání určitého uživatele z určité role. Na druhé stránce jsme se rozšířit ovládacím prvku CreateUserWizard tak, že obsahují krok k určení, jaké role nově vytvořený uživatel patří. To je užitečné v situacích, kde je možné vytvořit nové uživatelské účty správce.


## <a name="introduction"></a>Úvod

<a id="_msoanchor_1"> </a> [Předchozí kurz o službě](creating-and-managing-roles-vb.md) prozkoumat rozhraní role a `SqlRoleProvider`; jsme viděli, jak používat `Roles` třída pro vytvoření, získání a odstranění rolí. Vedle vytváření a odstraňování role, potřebujeme mít možnost přiřazení nebo odebrání uživatele z role. Bohužel ASP.NET nedodává s webové ovládací prvky pro správu, které uživatelé patří do rolích. Místo toho jsme musíte vytvořit vlastní stránky technologie ASP.NET k spravují tyto asociace. Dobrou zprávou je, že přidání a odebrání uživatelů k rolím je poměrně snadné. `Roles` Třída obsahuje několik metod pro přidání jednoho nebo více uživatelů do jedné nebo více rolí.

V tomto kurzu vytvoříme dvě stránky technologie ASP.NET pro pomoc se správou, které uživatelé patří do rolích. První stránka bude obsahovat zařízení, pokud chcete zobrazit, co uživatelé patří do dané role, ke kterým rolím konkrétní uživatel patří do a možnost přiřazení nebo odebrání určitého uživatele z určité role. Na druhé stránce jsme se rozšířit ovládacím prvku CreateUserWizard tak, že obsahují krok k určení, jaké role nově vytvořený uživatel patří. To je užitečné v situacích, kde je možné vytvořit nové uživatelské účty správce.

Pusťme se do práce!

## <a name="listing-what-users-belong-to-what-roles"></a>Výpis uživatelé patří do jaké role

První pro účely tohoto kurzu je vytvořit webovou stránku, ze kterého můžete uživatele přiřadit k rolím. Předtím, než jsme si problém s přiřazení uživatelů k rolím, Pojďme nejprve soustředit se na tom, jak zjistit, co uživatelé patří do rolích. Existují dva způsoby, jak zobrazit tyto informace: "role" nebo ""uživatel. Jsme by mohlo znamenat musí návštěvníka vyberte roli a pak je zobrazit všichni uživatelé, které patří k roli ("podle role" zobrazení) nebo jsme může vyzvat návštěvníka vyberte uživatele a pak je zobrazit role přiřazené k tomuto uživateli ("uživatelem" zobrazení).

Zobrazení "podle role" je užitečné v případech kdy chce vědět, sady, které uživatelům patřícím do určité role; návštěvníka zobrazení "uživatelem" je ideální pro návštěvníka potřebuje vědět, zejména uživatelské role. Dopřejeme si naši stránku zahrnují jak "role" a "user" rozhraní.

Začneme s vytvářením rozhraní "uživatelem". Toto rozhraní se skládají z rozevíracího seznamu a seznam zaškrtávacích políček. Rozevíracím seznamu naplní skupinu uživatelů v systému. Zaškrtávací políčka vyčíslí role. Kliknutím na uživatele z rozevíracího seznamu zkontroluje těchto rolí, které uživatel patří. Osoba, na stránce následně zaškrtněte nebo zrušte zaškrtnutí políček pro přidání nebo odebrání vybraného uživatele z odpovídající role.

> [!NOTE]
> Pomocí rozevíracího seznamu pro seznam uživatelských účtů není ideální volbou pro websites tam, kde může být stovky uživatelských účtů. Rozevírací seznam je navržena k umožnění uživateli vybrat jednu z položek v poměrně krátké seznam možností. To rychle nepraktický roste počet položek seznamu. Pokud sestavujete web, který bude mít potenciálně velkého počtu uživatelských účtů, můžete zvážit použití alternativní uživatelským rozhraním, jako je stránkované GridView nebo filterable rozhraní, které jsou uvedeny vyzve návštěvníka zvolit písmenem a pak pouze zobrazí uživatelé, jejichž uživatelské jméno začíná vybrané písmeno.


## <a name="step-1-building-the-by-user-user-interface"></a>Krok 1: Vytvoření uživatelského rozhraní "Uživatelem"

Otevřít `UsersAndRoles.aspx` stránky. V horní části stránky, přidejte ovládací prvek popisek Web s názvem `ActionStatus` a vymažte její `Text` vlastnost. Tento popisek použije k poskytnutí zpětné vazby na akce prováděné, zobrazování zpráv, jako jsou, "Tito uživatele byl přidán do Administrators role" nebo "Jisun uživatele byl odebrán z role správců." Pokud chcete tyto zprávy odlišit, nastavte jeho `CssClass` vlastnost "Důležitá".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Dále přidejte následující definice třídy šablony stylů CSS do `Styles.css` šablony stylů:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Tato definice šablony stylů CSS dostane pokyn, aby zobrazení pomocí velké, červenou písma popisku. Obrázek 1 ukazuje tento efekt prostřednictvím návrháře Visual Studio.


[![Vlastnosti popisku CssClass výsledkem písmo velké, Red](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Obrázek 1**: The popisek `CssClass` vlastnost za následek velký, písmo Red ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image3.png))


Dále přidejte DropDownList na stránku, nastavte jeho `ID` vlastnost `UserList`a nastavte jeho `AutoPostBack` vlastnost na hodnotu True. Použijeme tuto DropDownList seznam všech uživatelů v systému. Tato DropDownList bude vázán k kolekce objektů MembershipUser. Protože chceme, aby DropDownList zobrazit Vlastnost UserName objekt MembershipUser (a použít ji jako hodnotu položky seznamu), nastavte DropDownList `DataTextField` a `DataValueField` vlastnosti "UserName".

Pod DropDownList, přidejte opakovače s názvem `UsersRoleList`. Tato Repeater zobrazí seznam všech rolí v systému jako řadu objektů zaškrtávací políčka. Definování Repeateru `ItemTemplate` pomocí následující kód:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

`ItemTemplate` Kód obsahuje jeden ovládací prvek zaškrtávací políčko Web s názvem `RoleCheckBox`. Na zaškrtávací políčko `AutoPostBack` je nastavena na hodnotu True a `Text` vlastnost je vázána na `Container.DataItem`. Z důvodu syntaxe vázání dat je jednoduše `Container.DataItem` totiž rozhraní role vrátí seznam názvů rolí jako pole řetězců, a je toto pole řetězců, které jsme se vazba na opakovače. Důkladné popis Proč tato syntaxe je použita k zobrazení obsahu pole svázána s ovládacím prvkem webových dat je nad rámec tohoto kurzu. Další informace o této věci, předložit [skalární pole vazby na ovládací prvek webových dat](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

V tomto okamžiku "uživatelem" rozhraní v deklarativní by měl vypadat nějak takto:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Nyní jsme připraveni napsat kód k vytvoření vazby sadu uživatelských účtů do DropDownList a sadu rolí pro opakovače. V třídě modelu code-behind na stránce, přidejte metodu s názvem `BindUsersToUserList` a druhou s názvem `BindRolesList`, pomocí následujícího kódu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

`BindUsersToUserList` Metoda načte všechny uživatelské účty v systému prostřednictvím [ `Membership.GetAllUsers` metoda](https://msdn.microsoft.com/library/dy8swhya.aspx). Tím se vrátí [ `MembershipUserCollection` objekt](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), což je kolekce [ `MembershipUser` instance](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Tato kolekce je pak vázán `UserList` DropDownList. `MembershipUser` Instance tuto strukturu kolekce obsahují celou řadu vlastností, jako je `UserName`, `Email`, `CreationDate`, a `IsOnline`. Aby bylo možné dáte pokyn, aby DropDownList k zobrazení hodnoty `UserName` vlastnost, ujistěte se, že `UserList` společnosti DropDownList `DataTextField` a `DataValueField` byly nastaveny vlastnosti "UserName".

> [!NOTE]
> `Membership.GetAllUsers` Metoda má dvě přetížení: jednu, která nepřijímá žádné vstupní parametry a vrátí všechny uživatele a jednu, která přijímá celočíselné hodnoty pro index stránky a velikost stránky a vrátí pouze zadaný dílčí sadu uživatelů. Když existuje velké množství uživatelské účty se zobrazí v prvku stránkované uživatelského rozhraní, druhé přetížení lze efektivněji procházení uživatele vzhledem k tomu, že vrátí jenom přesné podmnožině uživatelské účty a ne všechny z nich.


`BindRolesToList` Metoda začíná voláním `Roles` třídy [ `GetAllRoles` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), která vrací pole řetězců obsahující role v systému. Tato pole řetězců je pak vázán na opakovače.

Nakonec musíme tyto dvě metody volat při prvním načtení stránky. Přidejte následující kód, který `Page_Load` obslužné rutiny události:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

S tímto kódem na místě věnujte chvíli najdete na stránce prostřednictvím prohlížeče; vaše obrazovka by měla vypadat podobně jako na obrázku 2. Všechny uživatelské účty, naplní se v rozevíracím seznamu a pod, každá role se zobrazí jako zaškrtávací políčko. Protože jsme nastavili `AutoPostBack` vlastnosti DropDownList a vlastnost CheckBoxes na hodnotu True, změna vybraného uživatele nebo kontrola nebo zrušíte zaškrtnutí role vyvolá zpětné volání. Neprovede se žádná akce, ale protože musíme ještě napište kód pro zpracování těchto akcí. Jsme budete řešit tyto úlohy v následujících dvou částech.


[![Na stránce zobrazí uživatelé a role](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Obrázek 2**: na stránce zobrazí uživatelé a role ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Kontrola role vybraného uživatele patří do

Při prvním načtení stránky nebo pokaždé, když návštěvníka nový uživatel vybere z rozevíracího seznamu, musíme aktualizovat `UsersRoleList`společnosti zaškrtávací políčka tak, aby dané role zaškrtávací políčko zaškrtnuto, pouze v případě, že vybraný uživatel patří do této role. K tomu vytvořit metodu s názvem `CheckRolesForSelectedUser` následujícím kódem:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

Výše uvedený kód spustí tak, že určíte, který je vybraný uživatel. Pak používá třídu role [ `GetRolesForUser(userName)` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) vrátit zadaného uživatele sadu rolí jako pole řetězců. Dále jsou uvedené položky Opakovači a každá položka `RoleCheckBox` zaškrtávacího políčka se odkazuje prostřednictvím kódu programu. Zaškrtávací políčko je zaškrtnuto, pouze v případě, že je součástí role odpovídá `selectedUsersRoles` pole řetězců.

> [!NOTE]
> `Linq.Enumerable.Contains(Of String)(...)` Syntaxe nebude kompilovat, pokud používáte technologii ASP.NET verze 2.0. `Contains(Of String)` Metoda je součástí [LINQ knihovny](http://en.wikipedia.org/wiki/Language_Integrated_Query), což je nová technologie ASP.NET 3.5. Pokud stále používáte technologii ASP.NET verze 2.0, použijte [ `Array.IndexOf(Of String)` metoda](https://msdn.microsoft.com/library/eha9t187.aspx) místo.


`CheckRolesForSelectedUser` Metoda musí být volána ve dvou případech: při prvním načtení stránky a pokaždé, když `UserList` změně vybraného indexu DropDownList společnosti. Proto volání této metody z `Page_Load` obslužné rutiny události (po volání `BindUsersToUserList` a `BindRolesToList`). Také, vytvořit obslužnou rutinu události pro DropDownList `SelectedIndexChanged` událostí a volání této metody z něj.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

S tímto kódem na místě můžete otestovat stránky v prohlížeči. Nicméně od verze `UsersAndRoles.aspx` stránky v tuto chvíli nemá schopnost přiřazení uživatelů k rolím, uživatelé nemají role. Vytvoříme rozhraní pro přiřazování uživatelů do rolí ve chvíli, abyste mohli Moje slovo, které tento kód funguje a ověřit, že je spuštění provedeno později, nebo můžete ručně přidat uživatele k rolím vložením záznamů do `aspnet_UsersInRoles` tabulky účely otestování tohoto functi Nyní onality.

### <a name="assigning-and-removing-users-from-roles"></a>Přiřazení a odebrání uživatele z role

Když návštěvníka kontroluje nebo zruší tohoto systému zaškrtnutí zaškrtávacího políčka v `UsersRoleList` Repeater potřebujeme přidat nebo odebrat vybraného uživatele z odpovídající roli. Na zaškrtávací políčko `AutoPostBack` je aktuálně nastavena na hodnotu True, což způsobí, že postback kdykoli v Opakovači zaškrtávací políčko je zaškrtnuté nebo nezaškrtnuté. Stručně řečeno, potřebujeme vytvořit obslužnou rutinu události pro na zaškrtávací políčko `CheckChanged` událostí. Protože zaškrtávacího políčka v ovládacím prvku Repeater, budeme potřebovat ručně přidat obslužnou rutinu události zajistí funkčnost systému. Začněte přidáním obslužné rutiny události k použití modelu code-behind třídu jako `Protected` metody takto:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Vrátíme psát kód pro tuto obslužnou rutinu události za chvíli. Nejdřív ukončeme vložení zpracování událostí. Od zaškrtávacího políčka v rámci prvku Repeater `ItemTemplate`, přidejte `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Vedení této syntaxe vodiče `RoleCheckBox_CheckChanged` obslužnou rutinu události `RoleCheckBox`společnosti `CheckedChanged` událostí.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

Naše poslední úloha je k dokončení `RoleCheckBox_CheckChanged` obslužné rutiny události. Musíme začít pomocí odkazu na ovládací prvek CheckBox, která vyvolala událost, protože tato instance zaškrtávací políčko nám řekne, jakou roli bylo zaškrtnuté nebo nezaškrtnuté prostřednictvím jeho `Text` a `Checked` vlastnosti. Na základě těchto informací spolu s uživatelské jméno vybraného uživatele jsme přidat nebo odebrat uživatele z role prostřednictvím `Roles` třídy [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) nebo [ `RemoveUserFromRole` metoda](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

Výše uvedený kód spustí programově odkazováním na zaškrtávací políčko, která vyvolala událost, která je k dispozici prostřednictvím `sender` vstupního parametru. Pokud je zaškrtnuté políčko, vybraný uživatel přidán do zadané roli, jinak se odeberou z role. V obou případech `ActionStatus` popisek zobrazí zprávu sumarizace akce teď udělali.

Za chvíli otestovat na této stránce prostřednictvím prohlížeče. Vyberte uživatele Tito a pak přidejte Tito do role správců a správců.


[![Byla přidána tito správci a správci rolí](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Obrázek 3**: byla přidána Tito správci a správci rolí ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image9.png))


V dalším kroku vyberte uživatele Bruce z rozevíracího seznamu. Je zpětné volání a zaškrtávací políčka Repeateru jsou aktualizovány pomocí `CheckRolesForSelectedUser`. Protože Bruce zatím nepatří k žádné roli, nekontrolované dvě zaškrtávací políčka. V dalším kroku přidejte Bruce k roli správců.


[![Bruce byl přidán do Role správců](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Obrázek 4**: Bruce byl přidán do Role správců ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image12.png))


Provést ještě další ověření funkce `CheckRolesForSelectedUser` metoda, vyberte uživatele, než jsou Tito a Bruce. Všimněte si, jak jsou automaticky není zaškrtnuto zaškrtávací políčka, které označuje, že nepatří k žádné roli. Vraťte se do Tito. Zaškrtávací políčka správců a správců by měly být porovnány.

## <a name="step-2-building-the-by-roles-user-interface"></a>Krok 2: Vytvoření uživatelského rozhraní "Rolemi"

V tuto chvíli jsme dokončili rozhraní "uživateli" a můžete začít řeší rozhraní "rolemi". Rozhraní "rolemi" vyzve uživatele k roli vyberte z rozevíracího seznamu a potom zobrazí sadu uživatelů, které patří do této role v GridView.

Přidejte další ovládací prvek DropDownList k `UsersAndRoles.aspx page`. Umístit tohohle pod ovládacím prvku opakovače, pojmenujte ho `RoleList`a nastavte jeho `AutoPostBack` vlastnost na hodnotu True. Pod, přidejte prvku GridView a pojmenujte ho `RolesUserList`. Tento prvek GridView zobrazí seznam uživatelů, které patří k vybrané roli. Nastavte prvku GridView `AutoGenerateColumns` vlastnost na hodnotu False, přidejte do mřížky TemplateField `Columns` kolekce a nastavte jeho `HeaderText` vlastnost "Uživatelům". Definovat TemplateField `ItemTemplate` tak, aby zobrazil hodnotu výraz datové vazby `Container.DataItem` v `Text` vlastnost popisek s názvem `UserNameLabel`.

Po přidání a konfigurace prvku GridView, "podle role" rozhraní v deklarativní by měl vypadat nějak takto:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Potřebujeme k naplnění `RoleList` DropDownList s sadu rolí v systému. Pokud chcete dosáhnout, aktualizujte `BindRolesToList` metodu tak, aby se vytvoří vazbu pole řetězce vrácené `Roles.GetAllRoles` metodu `RolesList` DropDownList (stejně jako `UsersRoleList` Repeater).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Poslední dva řádky v `BindRolesToList` metoda byly přidány do sadu rolí pro vytvoření vazby `RoleList` ovládací prvek DropDownList. Obrázek 5 ukazuje konečný výsledek při prohlížení prostřednictvím prohlížeče – v rozevíracím seznamu vyplní rolí v systému.


[![Role se zobrazí v RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Obrázek 5**: The role se zobrazí v `RoleList` DropDownList ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Zobrazení, které patří k vybrané roli uživatele

Při prvním načtení stránky, nebo když vyberete novou roli ze `RoleList` DropDownList, potřebujeme k zobrazení seznamu, které uživatelům patřícím do této role v prvku GridView. Vytvořit metodu s názvem `DisplayUsersBelongingToRole` pomocí následujícího kódu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Tato metoda začíná tím, že získáme vybranou roli z `RoleList` DropDownList. Poté použije [ `Roles.GetUsersInRole(roleName)` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) k načtení pole řetězců uživatelská jména uživatelů, které patří do této role. Toto pole je pak vázán `RolesUserList` ovládacího prvku GridView.

Tato metoda musí být volána ve dvou případech: při počátečním načtení stránky a při vybranou roli v `RoleList` DropDownList změny. Proto se aktualizace `Page_Load` obslužná rutina události tak, aby byla tato metoda vyvolána po volání `CheckRolesForSelectedUser`. Dále vytvořte obslužnou rutinu události pro `RoleList`společnosti `SelectedIndexChanged` události a tuto metodu volat z něj, příliš.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

S tímto kódem na místě `RolesUserList` GridView zobrazeno uživatelům, kteří patří do vybrané roli. Jak je vidět na obrázku 6, role správců se skládá ze dvou členů: Bruce a Tito.


[![Uvádí uživatele, kteří patří do vybrané Role prvku GridView.](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Obrázek 6**: The GridView obsahuje seznam těch, uživatelé, patří do vybrané Role ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Odstraňují se uživatelé z vybrané Role.

Můžeme rozšířit `RolesUserList` GridView tak, že obsahují sloupec "Remove" tlačítka. Kliknutím na tlačítko "Odebrat" pro konkrétního uživatele, budou odebrány z této role.

Začněte přidáním pole tlačítko Odstranit do prvku GridView. Ujistěte se, toto pole se zobrazí jako archivované nejvíce vlevo a změnit jeho `DeleteText` vlastnost "Odebrat" z "Odstranit" (výchozí).


[![Přidat](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Obrázek 7**: Přidání tlačítka "Odebrat" do prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image21.png))


Po kliknutí na tlačítko "Remove" vyplývá zpětné volání a prvku GridView `RowDeleting` událost se vyvolá. Potřebujeme vytvořit obslužnou rutinu události pro tuto událost a napsat kód, který odebere uživatele z vybrané role. Vytvořte obslužnou rutinu události a pak přidejte následující kód:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

Kód spustí tak, že určíte název vybrané role. Následně prostřednictvím kódu programu odkazy `UserNameLabel` ovládacího prvku z řádku došlo ke kliknutí na tlačítko jehož "Odebrat" aby bylo možné zjistit uživatelské jméno uživatele k odebrání. Uživatel pak odebrána z role prostřednictvím volání `Roles.RemoveUserFromRole` metody. `RolesUserList` GridView se potom aktualizují a zobrazí se zpráva prostřednictvím `ActionStatus` ovládacímu prvku popisek.

> [!NOTE]
> Pomocí tlačítka "Odebrat" nevyžaduje, aby jakýkoli druh potvrzení od uživatele před odebráním uživatele z role. Můžu pozvat k přidání určitou úroveň potvrzení uživatelem. Jedním z nejjednodušších způsobů k potvrzení akce je prostřednictvím dialogového okna potvrdit na straně klienta. Další informace o této techniky najdete v tématu [přidání Client-Side potvrzení při odstraňování](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Obrázek 8 ukazuje na stránku, jakmile uživatel Tito byl odebrán ze skupiny správců.


[![Jenže Tito už není nadřízeným](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Obrázek 8**: bohužel Tito už není nadřízeným ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Přidání nových uživatelů do vybrané Role.

Spolu se odstraňují se uživatelé z vybrané role, návštěvníky na tuto stránku by měla být schopni přidat uživatele k vybrané roli. Nejlepší rozhraní pro přidání uživatele k vybrané roli závisí na počet uživatelských účtů, které plánujete mít. Pokud váš web bude obsahovat pár desítek uživatelské účty nebo méně, můžete zde použít DropDownList. Můžou existovat tisíce uživatelské účty, byste zahrnují uživatelské rozhraní, která umožňuje návštěvníka na stránce pomocí účtů, vyhledání konkrétního účtu, nebo uživatelské účty v jiných nějak filtrovat.

Pro tuto stránku použijeme velmi jednoduché rozhraní, která funguje bez ohledu na počet uživatelských účtů v systému. Konkrétně budeme používat textové pole, zobrazení výzvy zadejte uživatelské jméno uživatele, kterého chce přidat do vybrané role musí návštěvníka. Pokud neexistuje žádný uživatel s tímto názvem, nebo pokud uživatel je již členem role, zobrazíme vám zpráva v `ActionStatus` popisek. Ale pokud uživatel existuje a není členem role, přidáme je do role a aktualizaci mřížky.

Přidání textového pole a tlačítko pod prvku GridView. Nastavit textové pole `ID` k `UserNameToAddToRole` a tlačítka nastavte `ID` a `Text` vlastností `AddUserToRoleButton` a "Přidání uživatele do Role", v uvedeném pořadí.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Dále vytvořte `Click` obslužné rutiny události pro `AddUserToRoleButton` a přidejte následující kód:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

Většina kódu v `Click` obslužná rutina události provádí různé ověřovací kontroly. Zajišťuje, že zadaný návštěvník s uživatelským jménem v `UserNameToAddToRole` textového pole, který uživatel v systému existuje a že již nepatří do vybrané roli. Pokud některý z těchto kontrol selže, zobrazí se odpovídající zprávu v `ActionStatus` a obslužná rutina události je byl ukončen. Pokud všechny kontroly úspěšně prošel zpracováním, uživatel je přidán k roli prostřednictvím `Roles.AddUserToRole` metody. Pod textovým polem pro `Text` vlastnost je odstraněné, aktualizaci prvku GridView a `ActionStatus` popisek zobrazí zprávu s upozorněním, že zadaný uživatel se úspěšně přidal do vybranou roli.

> [!NOTE]
> Pokud chcete mít jistotu, že zadaný uživatel již nepatří do vybrané role, používáme [ `Roles.IsUserInRole(userName, roleName)` metoda](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), která vrací logickou hodnotu označující, zda *uživatelské jméno* je členem skupiny *roleName*. Použijeme tuto metodu znovu v <a id="_msoanchor_2"> </a> [další kurz](role-based-authorization-vb.md) když se podíváme na ověřování na základě rolí.


Na stránce prostřednictvím prohlížeče a vyberte roli správců z `RoleList` DropDownList. Zkuste zadat neplatné uživatelské jméno – měla zobrazit zpráva s vysvětlením, že uživatel neexistuje v systému.


[![Nelze přidat neexistující uživatele k roli](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Obrázek 9**: nelze přidat neexistující uživatele k roli ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image27.png))


Nyní přidejte platného uživatele. Pokračujte a znovu přidejte Tito do role správců.


[![Tito je opět Supervisor!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Obrázek 10**: Tito je opět Supervisor!  ([Kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Krok 3: Více aktualizací "User" a "Role" rozhraní

`UsersAndRoles.aspx` Stránka nabízí dvě odlišné rozhraní pro správu uživatelů a rolí. V současné době tato dvě rozhraní fungují nezávisle na mezi sebou, takže je možné, že změny provedené v jednom rozhraní okamžitě neprojeví v jiném. Představte si například, že vybere návštěvníka na stránce role správců z `RoleList` DropDownList, které jsou uvedeny Bruce a Tito jako členy. V dalším kroku návštěvníka vybere Tito z `UserList` DropDownList, které slouží k ověření správců a správců zaškrtávací políčka v `UsersRoleList` opakovače. Pokud návštěvníka pak zruší tohoto systému zaškrtnutí roli vedoucí z opakovače, Tito je odebrán z role správců, ale tato změna se neprojeví v rozhraní "podle role". Prvku GridView, bude mít Tito jako členem role správců.

Chcete-li vyřešit to potřebujeme k aktualizaci prvku GridView. pokaždé, když se role je zaškrtnuté nebo nezaškrtnuté z `UsersRoleList` opakovače. Podobně musíme aktualizovat Opakovači pokaždé, když se uživatel se přidaly nebo odebraly roli z rozhraní "podle role".

Repeater v rozhraní "uživatelem" se aktualizují pomocí volání `CheckRolesForSelectedUser` metody. Rozhraní "podle role", lze upravit v `RolesUserList` prvku GridView `RowDeleting` obslužné rutiny události a `AddUserToRoleButton` tlačítka `Click` obslužné rutiny události. Proto potřebujeme k volání `CheckRolesForSelectedUser` metodu z každé z těchto metod.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Podobně se aktualizují GridView v rozhraní "podle role" voláním `DisplayUsersBelongingToRole` metoda a interface "uživatelem" je upravena `RoleCheckBox_CheckChanged` obslužné rutiny události. Proto potřebujeme k volání `DisplayUsersBelongingToRole` metodu z této obslužné rutiny události.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

S těmito změnami drobné úpravy v kódu "user" a "role" rozhraní nyní správně mezi update. Chcete-li to ověřit, najdete na stránce prostřednictvím prohlížeče a vyberte Tito a správců z `UserList` a `RoleList` DropDownLists, v uvedeném pořadí. Všimněte si, že jako zrušte zaškrtnutí políčka role správců pro Tito z Repeater v rozhraní "uživatelem" Tito se automaticky odebere z prvku GridView v rozhraní "podle role". Přidání Tito zpět do role správců z rozhraní "podle role" automaticky znovu ověří nadřízeným zaškrtávacího políčka v rozhraní "uživatelem".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Krok 4: Přizpůsobení CreateUserWizard zahrnout krok "Zadejte role"

V <a id="_msoanchor_3"> </a> [ *vytváření uživatelských účtů* ](../membership/creating-user-accounts-vb.md) kurzu jsme viděli, jak pomocí ovládacího prvku CreateUserWizard webové zajištění rozhraní pro vytvoření nového uživatelského účtu. Ovládacím prvku CreateUserWizard je možné v jednom ze dvou způsobů:

- Jako prostředek pro návštěvníky vytvořit vlastní uživatelský účet na webu a
- Jako prostředek pro správce vytvořit nové účty

V prvním případě použití návštěvník na web a vyplní CreateUserWizard, jejich údaje zadat-li se zaregistrovat na webu. V druhém případě správce vytvoří nový účet jiné osoby.

Když účet se vytváří microsoftem nebo správcem pro jinou osobu, může být užitečné, aby správce mohl zadat nový uživatelský účet patří ke kterým rolím. V <a id="_msoanchor_4"> </a> [ *ukládání* *Další informace o uživateli* ](../membership/storing-additional-user-information-vb.md) kurzu jsme viděli, jak přizpůsobit: můžete přidat další CreateUserWizard `WizardSteps`. Podívejme se na tom, jak přidat další krok CreateUserWizard zadejte nové uživatelské role.

Otevřít `CreateUserWizardWithRoles.aspx` stránce a přidání ovládacího prvku CreateUserWizard s názvem `RegisterUserWithRoles`. Nastavit u tohoto prvku `ContinueDestinationPageUrl` vlastnost "~ / Default.aspx". Protože zde spočívá, že správce budete používat tohoto ovládacího prvku CreateUserWizard k vytvoření nové uživatelské účty, nastavit u tohoto prvku `LoginCreatedUser` vlastnost na hodnotu False. To `LoginCreatedUser` vlastnost určuje, zda návštěvníka automaticky přihlášeni jako uživatel nově vytvořené a výchozí hodnota je True. Jsme to nastavili na hodnotu False vzhledem k tomu, když správce vytvoří nový účet my chceme zajistit mu přihlášení jako sám.

V dalším kroku vyberte "Přidat nebo odebrat `WizardSteps`..." z inteligentních značek CreateUserWizard a přidejte nový `WizardStep`a nastavte jeho `ID` k `SpecifyRolesStep`. Přesunout `SpecifyRolesStep WizardStep` tak, že jde o za krok "Sign k svůj nový účet", ale před krokem "Dokončených". Nastavte `WizardStep`společnosti `Title` vlastnost rolím"zadejte", jeho `StepType` vlastnost `Step`a jeho `AllowReturn` vlastnost na hodnotu False.


[![Přidat](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Obrázek 11**: přidání "Zadejte rolí" `WizardStep` k CreateUserWizard ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image33.png))


Po této změně vašeho CreateUserWizard deklarativní by měl vypadat nějak takto:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

V části "Určení role" `WizardStep`, přidejte CheckBoxList s názvem `RoleList.` tento CheckBoxList zobrazí seznam dostupných rolí, umožňuje uživateli na stránce zkontrolujte, jaké role nově vytvořený uživatel patří.

S dvě úlohy kódování ponechali jsme: nejprve musí naplníme `RoleList` CheckBoxList s rolemi v systému; za druhé, musíme přidat vytvořeného uživatele do vybrané role, když uživatel přesune z kroku "Zadejte role" krok "Dokončit". Můžeme provést první úkol v `Page_Load` obslužné rutiny události. Následující kód programově odkazy `RoleList` zaškrtávací políčko na první přejděte na stránku a sváže s rolí v systému k němu.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

Ve výše uvedeném kódu by měla vypadat povědomě. V <a id="_msoanchor_5"> </a> [ *ukládání* *Další informace o uživateli* ](../membership/storing-additional-user-information-vb.md) kurzu jsme použili dva `FindControl` příkazy k odkazování webový ovládací prvek z v rámci vlastní `WizardStep`. A kód, který se váže CheckBoxList role byla získána z dříve v tomto kurzu.

Aby bylo možné provést druhou úlohou programování potřebujeme vědět při dokončení kroku "Zadejte rolí". Vzpomínáte, který má CreateUserWizard `ActiveStepChanged` událost, která se spustí pokaždé, když návštěvníka přejde od jednoho kroku do druhého. Tady můžete určíme, pokud uživatel dosáhl krok "Dokončit". Pokud ano, potřebujeme přidat uživatele do vybraných rolí.

Vytvořte obslužnou rutinu události pro `ActiveStepChanged` událostí a přidejte následující kód:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Pokud uživatel právě dosáhl krok "Dokončeno", obslužná rutina události vytvoří výčet položky `RoleList` CheckBoxList a nově vytvořené uživatelem je přiřazen k vybrané role.

Navštivte tuto stránku prostřednictvím prohlížeče. Prvním krokem při CreateUserWizard je standardní krok "Sign k svůj nový účet", který zobrazí výzvu k zadání nové uživatelské jméno, heslo, e-mailu a další informace o klíči. Zadejte informace k vytvoření nového uživatele s názvem Wanda.


[![Vytvoření nového uživatele s názvem Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Obrázek 12**: Vytvořte nový účet uživatele s názvem Wanda ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image36.png))


Klikněte na tlačítko "Create User". CreateUserWizard interně volá `Membership.CreateUser` metoda vytvoření nového uživatelského účtu a pak pokračuje k dalšímu kroku, "Zadejte role." Tady jsou uvedené role systému. Zaškrtněte políčko nadřízeným a klikněte na tlačítko Další.


[![Ujistěte se, Wanda členem Role správců](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Obrázek 13**: Zkontrolujte Wanda členem Role správců ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image39.png))


Kliknutím na další způsobí, že zpětné volání a aktualizace `ActiveStep` na krok "Dokončit". V `ActiveStepChanged` obslužná rutina události, nedávno vytvořen uživatelský účet je přiřazen k roli správců. Chcete-li to ověřit, vraťte se na `UsersAndRoles.aspx` stránku a vybrat správců z `RoleList` DropDownList. Jak ukazuje obrázek 14 správců je nyní tvoří tři uživatele: Bruce, Tito a Wanda.


[![Bruce, Tito a Wanda jsou všech správců](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Obrázek 14**: Bruce, Tito a Wanda jsou všechny správců ([kliknutím ji zobrazíte obrázek v plné velikosti](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Souhrn

Architektura rolí nabízí metody pro načtení informací o rolích a metody pro určení, které uživatelé patří do zadané role konkrétního uživatele. Kromě toho existuje několik metod pro přidání a odebrání jednoho nebo více uživatelů k jedné nebo více rolím. V tomto kurzu jsme se zaměřili na právě dvě z těchto metod: `AddUserToRole` a `RemoveUserFromRole`. Existují další varianty navržené a přidávání více uživatelů do jedné role přiřadit víc rolí na jednoho uživatele.

Tento kurz obsahuje taky podívat na rozšíření ovládacím prvku CreateUserWizard zahrnout `WizardStep` k určení nově vytvořené uživatelské role. Takové krok může pomoct zjednodušit proces vytváření uživatelských účtů pro nové uživatele správce.

V tuto chvíli jsme viděli jak vytvářet a odstraňovat role a jak přidávat a odebírat uživatele z role. Ale musíme ještě podívejte se na použití ověřování na základě rolí. V <a id="_msoanchor_6"> </a> [následující kurz](role-based-authorization-vb.md) se podíváme na definování autorizačních pravidel adres URL na základě rolí role a jak omezit tak jeho funkčnost úrovni stránky založené na rolích aktuálně přihlášeného uživatele.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přehled nástroje pro správu webu technologie ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Zkoumání ASP. Členství, role a profilu sítě.](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Vlastní nástroj pro správu webu se zajištěním provozu](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k...

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](creating-and-managing-roles-vb.md)
> [další](role-based-authorization-vb.md)
