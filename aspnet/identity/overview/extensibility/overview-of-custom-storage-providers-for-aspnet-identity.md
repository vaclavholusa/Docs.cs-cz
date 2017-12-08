---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: "Přehled zprostředkovatelů vlastní úložiště pro identitu ASP.NET | Microsoft Docs"
author: tfitzmac
description: "ASP.NET Identity je rozšiřitelný systém díky tomu můžete vytvořit vlastního poskytovatele úložiště a zařadit ho do aplikace bez znovu práce aplika..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 1ea779cb10661512690e3fec16ae73be0f40d15a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Přehled zprostředkovatelů vlastní úložiště pro identitu ASP.NET
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity je rozšiřitelný systém díky tomu můžete vytvořit vlastního poskytovatele úložiště a zařadit ho do aplikace bez znovu práce aplikace. Toto téma popisuje postup vytvoření poskytovatele vlastní úložiště pro ASP.NET Identity. Pokrývá důležité koncepty pro vytvoření vlastního zprostředkovatele úložiště, ale není podrobný postup implementace vlastního poskytovatele úložiště.
> 
> Příklad implementace vlastního poskytovatele úložiště naleznete v části [implementace vlastního zprostředkovatele úložiště ASP.NET Identity MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Toto téma byl aktualizovaný pro technologii ASP.NET 2.0 Identity.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Visual Studio 2013 s aktualizací 2
> - ASP.NET Identity 2


## <a name="introduction"></a>Úvod

Ve výchozím nastavení ASP.NET Identity systému jsou uloženy informace o uživateli v databázi systému SQL Server a používá k vytvoření databáze Entity Framework Code First. Pro mnoho aplikací tento postup funguje dobře. Ale chcete použít jiný typ stálosti mechanismu, jako je Azure Table Storage, nebo už můžete mít tabulek databáze se strukturou velmi jiný než výchozí implementace. V obou případech můžete napsat vlastní zprostředkovatele pro mechanizmus pro ukládání a zařadit tohoto zprostředkovatele do vaší aplikace.

ASP.NET Identity je zahrnuté ve výchozím nastavení v mnoha šablony sady Visual Studio 2013. Aktualizace můžete získat na ASP.NET Identity prostřednictvím [balíček Microsoft AspNet Identity EntityFramework NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Toto téma obsahuje následující části:

- [Pochopení architektura](#architecture)
- [Pochopení data, která je uložená](#data)
- [Vytvoření vrstvy přístupu k datům](#dal)
- [Přizpůsobit třídu uživatelů](#user)
- [Přizpůsobení úložiště uživatele](#userstore)
- [Přizpůsobení role – třída](#role)
- [Přizpůsobení úložiště rolí](#rolestore)
- [Překonfigurujte aplikace pro použití nového poskytovatele úložiště](#reconfigure)
- [Jiné implementace zprostředkovatelů vlastní úložiště](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Pochopení architektura

ASP.NET Identity se skládá z třídy jako správce a úložiště. Správci jsou základní třídy, které vývojář aplikace používá k provádění operací, jako je například vytváření uživatele v identitě ASP.NET Identity systému. Úložiště jsou třídy nižší úrovně, které určují, jak jsou entity, jako jsou uživatelé a role, nastavené jako trvalé. Úložiště jsou úzce kombinaci s mechanismu trvalosti, ale správci jsou odpojené od úložiště, což znamená, že nahradíte mechanismu trvalosti bez přerušení v celé aplikaci.

Následující diagram znázorňuje, jak webová aplikace komunikuje s vybraných manažerů a ukládá interakci s vrstva přístupu k datům.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Chcete-li vytvořit poskytovatele vlastní úložiště pro ASP.NET Identity, budete muset vytvořit zdroj dat, vrstva přístupu k datům a třídy úložiště, které pracují s Tato vrstva. Můžete nadále používat stejný manager rozhraní API k provádění operací s daty na uživatele, ale teď, když jsou data uložena do jiného úložiště systému.

Není nutné přizpůsobit třídy manager, protože při vytváření novou instanci třídy objekt UserManager nebo RoleManager můžete zadat typ třídy uživatelů a předat jako argument instance třídy úložiště. Tento přístup umožňuje zařadit vaše vlastní třídy do stávající struktury. Uvidíte, jak se vaše vlastní úložiště třídy v části vytvořit instanci nastavení UserManager a RoleManager [překonfigurovat aplikace pro použití nového poskytovatele úložiště](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Pochopení data, která je uložená

K implementaci vlastního poskytovatele úložiště, musíte pochopit typy dat použít s ASP.NET Identity a rozhodnout, funkce, které jsou relevantní pro vaše aplikace.

| Data | Popis |
| --- | --- |
| Uživatelé | Registrovaní uživatelé vašeho webu. Zahrnuje uživatelské Id a uživatelské jméno. Pokud se uživatelé přihlašovat pomocí přihlašovacích údajů, které jsou specifické pro váš web může obsahovat hodnoty hash hesla (nikoli pomocí přihlašovacích údajů z externího webu, jako je Facebook) a razítko zabezpečení pro určení, zda nic byla změněna přihlašovací údaje uživatele. Může také obsahovat e-mailovou adresu, telefonní číslo, zda je povoleno dvoufaktorové ověřování, aktuální počet neúspěšných přihlášení, a zda je účet uzamčen. |
| Deklarace identity uživatelů | Sada příkazů (nebo deklarace identity) o uživatele, který reprezentuje identitu uživatele. Můžete povolit větší výraz identitu uživatele, než lze dosáhnout pomocí rolí. |
| Přihlášení uživatele | Informace o zprostředkovateli externího ověřování (jako je Facebook) pro použití při přihlášení uživatele. |
| Role | Skupiny autorizaci pro svůj web. Obsahuje název role a Id role (například "Admin" nebo "Zaměstnanec"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Vytvoření vrstvy přístupu k datům

Toto téma předpokládá, že jste obeznámeni s trvalost mechanismus, který chcete použít a jak vytvořit entit pro tento mechanismus. Toto téma neobsahuje podrobnosti o tom, jak vytvořit úložiště nebo data přístupové třídy; Namísto toho poskytuje několik návrhů o rozhodnutí o návrhu, že budete muset udělat při práci s ASP.NET Identity.

Máte spoustu svobodu při návrhu úložiště pro přizpůsobený zprostředkovatele úložiště. Potřebujete vytvořit úložiště pro funkce, které chcete používat ve vaší aplikaci. Například pokud nepoužíváte role v aplikaci, není potřeba vytvořit úložiště pro role nebo role uživatele. Technologie a existující infrastruktura může vyžadovat strukturu, která je příliš neliší od výchozí implementaci ASP.NET Identity. Přístup k vaší Datová vrstva poskytnete logiku pro práci s strukturu vašeho úložiště.

MySQL čerpat úložišť dat pro technologii ASP.NET 2.0 Identity, najdete v části [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Vrstva přístupu k datům poskytnete logika pro uložení dat do zdroje dat z ASP.NET Identity. Vrstva přístupu k datům pro poskytovatele vlastní úložiště může obsahovat následující třídy k ukládání informací o uživateli a role.

| Třída | Popis | Příklad |
| --- | --- | --- |
| Kontext | Zapouzdří informace pro připojení k vaší mechanismu trvalosti a spouštět dotazy. Tato třída je důležitá pro vaše vrstvou. Datové třídy bude vyžadovat instance této třídy lze provádět své operace. Bude také provést inicializaci třídy úložiště s instancí této třídy. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Úložiště uživatele | Ukládá a načte informace o uživateli (například uživatelské jméno a heslo hash). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Role úložiště | Ukládá a načte informace o rolích (například název role). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Úložiště objektu UserClaims. | Ukládá a načte informace o deklaraci identity uživatele (například typ deklarace identity a hodnota). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Sada úložiště | Ukládá a načte přihlašovací informace uživatele (například externí zprostředkovatel ověřování). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Položka UserRole úložiště | Ukládá a načte jednotlivé role uživatele je přiřazena k. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Znovu stačí k implementaci třídy, které chcete používat ve vaší aplikaci.

V datových tříd přístup zadejte kód pro provádění operací s daty pro vaše konkrétní trvalost mechanismus. Například v rámci implementace MySQL UserTable – třída obsahuje metody pro vložení nového záznamu do databázové tabulky uživatelů. Proměnné s názvem `_database` je instance třídy MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Po vytvoření přístupové třídy vaše data, musíte vytvořit třídy úložiště, které volání konkrétní metody v vrstva přístupu k datům.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Přizpůsobit třídu uživatelů

Při implementaci vlastního zprostředkovatele úložiště, je nutné vytvořit třídu uživatelů, která je ekvivalentní [IdentityUser](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) třídy v [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) obor názvů:

Následující diagram znázorňuje IdentityUser třídu, která je nutné vytvořit a rozhraní k implementaci v této třídě.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613291(v=vs.108).aspx) rozhraní definuje vlastnosti, které objekt UserManager pokusí volat při provádění požadované operace. Rozhraní obsahuje dvě vlastnosti - Id a uživatelského jména. [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613291(v=vs.108).aspx) rozhraní umožňuje určit typ klíče pro uživatele prostřednictvím obecná **TKey** parametr. Typ vlastnosti Id odpovídá hodnotě parametru TKey.

Také poskytuje rozhraní Identity [IUser](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) rozhraní (bez obecný parametr) Pokud chcete pro klíč použít hodnotu řetězce.

Třída IdentityUser implementuje IUser a obsahuje všechny další vlastnosti nebo konstruktory pro uživatele na váš web z umístění. Následující příklad ukazuje třídu IdentityUser používající celé klíče. Pole Id je nastaveno **int** odpovídat hodnotě obecný parametr. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Pro úplnou implementaci, najdete v části [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Přizpůsobení úložiště uživatele

Můžete také vytvořit úložiště UserStore třídu, která poskytuje metody pro všechny operace data na uživatele. Tato třída je ekvivalentní [objektu UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/en-us/library/dn315446(v=vs.108).aspx) třídy v [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) oboru názvů. V třídě úložiště UserStore implementovat [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613276(v=vs.108).aspx) a všechny volitelné rozhraní. Vyberete které volitelné rozhraní k implementaci na základě na funkce, které chcete zajistit ve vaší aplikaci.

Následující obrázek ukazuje třídy objektu UserStore, na kterou je nutné vytvořit a relevantní rozhraní.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Výchozí šablona projektu v sadě Visual Studio obsahuje kód, který předpokládá, že mnoho volitelné rozhraní je implementovaná v úložišti uživatele. Pokud používáte výchozí šablony s úložištěm přizpůsobené uživatele, musíte implementovat volitelné rozhraní v úložišti uživatele nebo změnit kód šablony už volání metod v rozhraní, které ještě implementována.

 Následující příklad ukazuje třídu úložiště jednoduché uživatele. **TUser** obecný parametr trvá typ třídy vaše uživatele, který je obvykle třídě IdentityUser jste definovali. **TKey** obecný parametr trvá typ uživatelský klíč. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 V tomto příkladu konstruktor, který přebírá parametr s názvem *databáze* typu ExampleDatabase je pouze ilustraci jak předat ve třídě data access. Tento konstruktor přijímá implementace MySQL, parametr typu MySQLDatabase. 

V třídě úložiště UserStore použijete přístupové třídy dat, které jste vytvořili k provádění operací. Třída objektu UserStore má k implementaci MySQL asynchronně metoda, která používá instanci UserTable vložit nový záznam. **Vložit** metodu **userTable** objekt se stejnou metodu, která se zobrazí v předchozí části. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Rozhraní k implementaci při přizpůsobení úložiště uživatele

Následující obrázek ukazuje další informace o funkcích, které jsou definované v každé rozhraní. Všechny volitelné rozhraní dědit z úložiště IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **Úložiště IUserStore**  
 [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613278(v=vs.108).aspx) rozhraní je pouze rozhraní musí implementovat v úložišti uživatele. Definuje metody pro vytváření, aktualizaci, odstranění a načítání uživatelů.
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613265(v=vs.108).aspx) rozhraní definuje metody, je nutné implementovat v úložišti uživatele povolující deklarace identity uživatele. Obsahuje metody nebo přidání, odebrání a načítání deklarace identity uživatelů.
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613272(v=vs.108).aspx) definuje metody, je nutné implementovat v úložišti uživatele povolit externí zprostředkovatele ověřování. Obsahuje metody pro přidání, odebrání a načítání přihlášení uživatele a metodu pro načtení uživatele podle přihlašovacích údajů.
- **IUserRoleStore**  
 [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/en-us/library/dn613276(v=vs.108).aspx) rozhraní definuje metody, je nutné implementovat v úložišti pro mapování uživatele k roli uživatele. Obsahuje metody pro přidání, odebrání a načíst role uživatele a umožňuje kontrolu, pokud uživatel je přiřazena k roli.
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613273(v=vs.108).aspx) rozhraní definuje metody, je nutné implementovat v úložišti uživatele k uchování použita hodnota hash hesla. Obsahuje metody pro získání a nastavení hodnoty hash hesla a metodu, která označuje, zda má uživatel nastavit heslo.
- **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613277(v=vs.108).aspx) rozhraní definuje metody, je nutné implementovat v úložišti uživatele používat razítko zabezpečení pro označující, zda došlo ke změně informací o účtu uživatele . Razítko se aktualizuje, když uživatel změní heslo, nebo přidá nebo odebere přihlášení. Obsahuje metody pro získání a nastavení razítko zabezpečení.
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613279(v=vs.108).aspx) rozhraní definuje metody, je nutné implementovat pro implementace dvoufaktorové ověřování. Obsahuje metody pro získání a nastavení, zda je pro uživatele povoleno dvoufaktorové ověřování.
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613275(v=vs.108).aspx) rozhraní definuje metody, je nutné implementovat pro uložení telefonních čísel uživatele. Obsahuje metody pro získání a nastavení telefonní číslo a zda telefonní číslo je potvrzeno.
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613143(v=vs.108).aspx) rozhraní definuje metody, je nutné implementovat pro uložení e-mailové adresy uživatele. Obsahuje metody pro získání a nastavení e-mailovou adresu a zda e-mail je potvrzen.
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613271(v=vs.108).aspx) rozhraní definuje metody, je nutné implementovat k uložení informací o uzamčení účtu. Obsahuje metody pro získání aktuální počet neúspěšných pokusů o přístup, získání a nastavení, zda účet může být uzamčen, získání a nastavení uzamčení koncové datum, zvyšování počet neúspěšných pokusů o a resetuje počet neúspěšných pokusů o přihlášení.
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613267(v=vs.108).aspx) rozhraní definuje členy, je nutné implementovat zajistit dotazovatelné úložiště uživatelů. Obsahuje vlastnost, která obsahuje dotazovatelné uživatele.

 Implementace rozhraní, které jsou potřeba v aplikaci; IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore a IUserSecurityStampStore rozhraní, jako, jak je uvedeno níže. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Pro dokončení implementace (včetně všech rozhraní), najdete v části [objektu UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin a IdentityUserRole

Obor názvů Microsoft.AspNet.Identity.EntityFramework obsahuje implementace [IdentityUserClaim](https://msdn.microsoft.com/en-us/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/en-us/library/dn613251(v=vs.108).aspx), a [IdentityUserRole](https://msdn.microsoft.com/en-us/library/dn613252(v=vs.108).aspx) třídy. Pokud používáte tyto funkce, můžete vytvořit vlastní verzích tyto třídy a definovat vlastnosti pro vaši aplikaci. V některých případech je však efektivnější zatížení tyto entity do paměti při provádění základních operací (například přidáním nebo odebráním deklarace identity uživatele). Třídy úložiště back-end místo toho můžete tyto operace přímo na zdroj dat provést. Například metoda UserStore.GetClaimsAsync() můžete volat userClaimTable.FindByUserId(user. Metoda ID), při spuštění dotazu na, která přímo tabulce a vrátí seznam deklarací identity.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Přizpůsobení role – třída

Při implementaci vlastního zprostředkovatele úložiště, je nutné vytvořit třídu role, která je ekvivalentní [IdentityRole](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) třídy v [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) obor názvů:

Následující diagram znázorňuje IdentityRole třídu, která je nutné vytvořit a rozhraní k implementaci v této třídě.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613268(v=vs.108).aspx) rozhraní definuje vlastnosti, které RoleManager pokusů o volat při provádění požadované operace. Rozhraní obsahuje dvě vlastnosti - Id a název. [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613268(v=vs.108).aspx) rozhraní umožňuje určit typ klíče pro roli prostřednictvím obecná **TKey** parametr. Typ vlastnosti Id odpovídá hodnotě parametru TKey.

Také poskytuje rozhraní Identity [IRole](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) rozhraní (bez obecný parametr) Pokud chcete pro klíč použít hodnotu řetězce.

Následující příklad ukazuje třídu IdentityRole používající celé klíče. Pole Id je nastaveno na celá čísla odpovídají hodnotě obecný parametr. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Pro úplnou implementaci, najdete v části [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Přizpůsobení úložiště rolí

Můžete také vytvořit úložiště RoleStore třídu, která poskytuje metody pro všechny operace data na rolích. Tato třída je ekvivalentní [úložiště RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/en-us/library/dn468181(v=vs.108).aspx) – třída v oboru názvů Microsoft.ASP.NET.Identity.EntityFramework. V třídě úložiště RoleStore implementovat [úložiště IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613266(v=vs.108).aspx) a volitelně [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613262(v=vs.108).aspx) rozhraní.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Následující příklad ukazuje třídu úložiště rolí. Obecný parametr TRole trvá typ třídy vaše role, který je obvykle IdentityRole třídy, na kterou jste definovali. Obecný parametr TKey trvá typ klíče role. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **Úložiště IRoleStore&lt;TRole&gt;**  
 [Úložiště IRoleStore](https://msdn.microsoft.com/en-us/library/dn468195.aspx) rozhraní definuje metody k implementaci v třídě úložiště rolí. Obsahuje metody pro vytváření, aktualizaci, odstranění a načítání rolí.
- **Úložiště RoleStore&lt;TRole&gt;**  
 Chcete-li přizpůsobit úložiště RoleStore, vytvořte třídu, která implementuje rozhraní úložiště IRoleStore. Budete muset implementaci této třídy, pokud chcete použít role v systému. Konstruktor, který přebírá parametr s názvem *databáze* typu ExampleDatabase je pouze ilustraci jak předat ve třídě data access. Tento konstruktor přijímá implementace MySQL, parametr typu MySQLDatabase.  
  
 Pro úplnou implementaci, najdete v části [úložiště RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Překonfigurujte aplikace pro použití nového poskytovatele úložiště

Byla implementována svého nového poskytovatele úložiště. Nyní musíte nakonfigurovat aplikace k používání tohoto zprostředkovatele úložiště. Pokud výchozí zprostředkovatel úložiště je zahrnutý v projektu, musíte odebrat výchozího zprostředkovatele a nahraďte ji u svého poskytovatele.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Nahraďte výchozího zprostředkovatele úložiště v projektu MVC

1. V **spravovat balíčky NuGet** okně odinstalovat **Microsoft ASP.NET Identity EntityFramework** balíčku. Tento balíček můžete najít tak, že v balíčcích nainstalovaná Identity.EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png)Zobrazí se výzva, pokud chcete také odinstalovat Entity Framework. Pokud není nutné ho v dalších částí vaší aplikace, můžete ho odinstalovat.
2. V souboru IdentityModels.cs ve složce modely, odstranit nebo komentář **ApplicationUser** a **ApplicationDbContext** třídy. V aplikaci MVC můžete odstranit celý soubor IdentityModels.cs. V aplikaci webových formulářů odstranění dvě třídy, ale ujistěte se, že necháte pomocná třída, která se nachází v souboru IdentityModels.cs.
3. Pokud poskytovatel úložiště se nachází v samostatné projekt, přidejte odkaz na jeho ve webové aplikaci.
4. Nahraďte všechny odkazy na `using Microsoft.AspNet.Identity.EntityFramework;` s pomocí příkazu pro obor názvů poskytovatele úložiště.
5. V **Startup.Auth.cs** třídy, změňte **ConfigureAuth** metodu použít jednu instanci odpovídající kontext. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. V aplikaci\_spouštěcí složka, otevřete **IdentityConfig.cs**. Ve třídě ApplicationUserManager změnit **vytvořit** metoda vrátí uživatele správce, který používá vaše přizpůsobené uživatelské úložiště. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Nahraďte všechny odkazy na **ApplicationUser** s **IdentityUser**.
8. Výchozí projekt obsahuje několik členů v třídu uživatelů, které nejsou definovány v rozhraní IUser; například e-mailu, PasswordHash a GenerateUserIdentityAsync. Pokud třídě user nemá tito členové, musíte buď jejich implementaci, nebo změňte kód, který používá tyto členy.
9. Pokud jste vytvořili všechny instance RoleManager, tento kód používat nové úložiště RoleStore třídy změňte.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Výchozí projekt je určená pro uživatele třídy, která má hodnotu řetězce pro klíč. Pokud třídě user má jiný typ klíče (například celé číslo), musíte změnit projektu pro práci s vašeho typu. V tématu [změnit primární klíč pro uživatele v identitě ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. V případě potřeby přidáte připojovací řetězec v souboru Web.config.

<a id="other"></a>
## <a name="other-resources"></a>Další zdroje

- Blog: [implementace ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Kurz a GIT kód: [zprostředkovatele Identity Simple.Data Asp.Net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Kurz:[nastavení účtů pro základní Identity a jejich odkazující na externí DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Podle [ @xivSolutions ](https://twitter.com/xivSolutions).
- Kurz[: implementace poskytovatele úložiště ASP.NET Identity vlastní MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entity CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) podle [SoftFluent](http://www.softfluent.com/)
- [Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) podle James Novák.
- Azure Table Storage: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) podle [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant podle Wertheim ADAM.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elastické Hle[h: elastické Identity](https://github.com/bmbsqd/elastic-identity) podle Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) podle Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) podle Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) podle [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) podle [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Šablony T4 ke generování EF code pro úložiště uživatele "databáze nejprve": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
