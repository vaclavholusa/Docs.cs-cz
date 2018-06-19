---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Členství | Microsoft Docs
author: microsoft
description: Členství technologie ASP.NET je založený na úspěch model ověřování formulářů z ASP.NET 1.x. Ověřování pomocí formulářů technologie ASP.NET poskytuje pohodlný způsob, jak incorp...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885561"
---
<a name="membership"></a>Členství
====================
podle [Microsoft](https://github.com/microsoft)

> Členství technologie ASP.NET je založený na úspěch model ověřování formulářů z ASP.NET 1.x. Ověřování pomocí formulářů technologie ASP.NET poskytuje pohodlný způsob, jak začlenit přihlašovacího formuláře do vaší aplikace ASP.NET a ověření uživatele na základě databáze nebo jiného úložiště dat.


Členství technologie ASP.NET je založený na úspěch model ověřování formulářů z ASP.NET 1.x. Ověřování pomocí formulářů technologie ASP.NET poskytuje pohodlný způsob, jak začlenit přihlašovacího formuláře do vaší aplikace ASP.NET a ověření uživatele na základě databáze nebo jiného úložiště dat. Členy třídy, ověřování pomocí formulářů umožňuje zpracovávat soubory cookie pro ověřování, zkontrolujte platného přihlášení, přihlásit uživatele se atd. Implementace ověřování pomocí formulářů v aplikaci ASP.NET 1.x může vyžadovat správného množství kódu.

Členství v technologii ASP.NET 2.0 je hlavní postupu oproti použití ověřování pomocí formulářů samostatně. (Členství je nejvíce robustní při kombinaci s ověřování pomocí formulářů, ale pomocí ověřování pomocí formulářů není povinné.) Jak brzy zjistíte, můžete použít členství technologie ASP.NET a ovládacích prvků přihlášení v technologii ASP.NET 2.0 implementace systému efektivní členství bez nutnosti psaní mnohem kódu vůbec.

## <a name="implementing-membership-in-aspnet-20"></a>Implementace členství technologie ASP.NET 2.0

Členství je implementováno modulem následující čtyři kroky. Mějte na paměti, že jsou mnoho dílčí kroků, které se podílejí a také volitelné konfigurace, který lze také implementovat. Tyto kroky jsou určené pro ilustraci velký obrázek konfigurace členství.

1. Vytvoření databáze členství (Pokud je SQL Server slouží jako úložiště členství.)
2. Zadejte možnosti, členství v konfiguračních souborech aplikace. (Členství je standardně povolená.)
3. Určuje typ úložiště členství, který chcete použít. Možnosti jsou: 

    - Microsoft SQL Server (verze 7.0 nebo novější)
    - Úložiště Active Directory
    - Vlastního poskytovatele členství
4. Konfigurace aplikace pro ověřování pomocí formulářů ASP.NET. Ještě jednou členství slouží k ověřování pomocí formulářů, výhod, ale pomocí ověřování pomocí formulářů není povinné.
5. Zadejte uživatelské účty pro členství a nakonfigurovat role v případě potřeby.

## <a name="creating-the-membership-database"></a>Vytvoření databáze členství

Pokud postup při použití SQL Server 7.0 nebo novější jako úložiště členství, můžete použít aspnet\_regsql nástroj (k dispozici snadno z Visual Studio .NET 2005 příkazového řádku) ke konfiguraci vaší databáze. Aspnet\_regsql se dá použít nástroj jako nástroj příkazového řádku nebo pomocí průvodce s grafickým uživatelským rozhraním. Metoda Průvodce je nejjednodušší způsob, jak nakonfigurovat databázi. Pro přístup k průvodci, jednoduše spusťte následující příkaz:

`aspnet_regsql W`

Po spuštění tohoto příkazu se zobrazí se Průvodce instalací serveru SQL pro ASP.NET jak je uvedeno níže.


![](membership/_static/image1.jpg)

**Obrázek 1**


Průvodce instalací serveru SQL pro ASP.NET vytvoří webovou stránku v instanci, kterou zadáte v průvodci. Ale technologie ASP.NET použije připojovací řetězec v souboru machine.config. pro připojení k vaší databázi. Ve výchozím nastavení, bude odkazovat tento připojovací řetězec k instanci systému SQL Server 2005, takže pokud používáte instanci systému SQL Server 2000 nebo SQL Server 7.0, budete muset upravit připojovací řetězec v souboru machine.config. Tento připojovací řetězec může být umístěn zde:

[!code-xml[Main](membership/samples/sample1.xml)]

Bohužel nemáte upravit připojovací řetězec, ASP.NET nebude získáte popisný chyby. Právě stěžovat si o tom, že jste nevytvořili databáze bude pokračovat. V případě výše I byly upraveny. připojovací řetězec tak, aby odkazoval na můj místní instanci systému SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Určení konfigurace a přidání uživatelů a rolí

Dalším krokem při konfiguraci členství je přidat informace potřebné k souboru web.config aplikace. Technologie ASP.NET 1.x, úprava souboru web.config se někdy složité z důvodu použití lowerCamelCase a nedostatek Intellisense. Visual Studio .NET 2005 zjednodušuje úlohu protože Intellisense pro konfigurační soubory, ale technologie ASP.NET 2.0 přejde ještě o krok napřed tím, že poskytuje webové rozhraní pro úpravu konfiguračních souborů.

Webové rozhraní můžete spustit kliknutím na tlačítko Konfigurace ASP.NET v nástrojů Průzkumníka řešení, jak je uvedeno níže. Můžete také spustit webové rozhraní prostřednictvím automaticky otevíraná okna, které se zobrazují, když jsou vloženy ovládací prvky pro přihlášení.


![](membership/_static/image2.jpg)

**Obrázek 2**


Spustí se nástroj Správa webu ASP.NET vidíte níže. Správa webu ASP.NET je čtyři karty rozhraní, které umožňuje snadno spravovat nastavení aplikace. K dispozici jsou následující karty:

- **Domovské**
- **Zabezpečení** konfigurovat uživatele, role a přístup.
- **Aplikace** nakonfigurovat nastavení aplikace.
- **Zprostředkovatel** konfigurace a testování vaší aplikace zprostředkovatele členství.

Nástroj pro správu webu umožňuje snadno vytvořit nové uživatele, vytvořte nové role a ke správě uživatelů a rolí. Tato možnost není k dispozici v rozhraní Windows. Rozhraní systému Windows můžete snadno definovat nastavení autorizace a přidání, odstranění a spravovat poskytovatelů, které nejsou v nástroji Správa webu.

Spuštění rozhraní systému Windows, otevřete modul snap-in služby IIS, klikněte pravým tlačítkem na aplikaci a vyberte možnost Vlastnosti. Klikněte na kartu ASP.NET a potom klikněte na tlačítko Upravit konfiguraci. (Aplikace musí být spuštěna v rámci technologie ASP.NET 2.0 pro tlačítko Upravit konfiguraci povolení. Verzi technologie ASP.NET můžete nakonfigurovat v dialogu ASP.NET.) Jak je uvedeno níže, zobrazí se dialogové okno nastavení konfigurace technologie ASP.NET.


![](membership/_static/image3.jpg)

**Obrázek 3**


Na kartě Obecné nastavení aplikace a připojovací řetězce jsou uvedené. Všechna nastavení kurzívou jsou definovány v nadřazeného konfiguračního souboru (souboru machine.config nebo web.config na vyšší úrovni) a nastavení není v kurzívě z konfiguračního souboru aplikace. Pokud je nastavení přidali, odebrat nebo upravit na úrovni aplikace ASP.NET bude přidat, odebrat nebo změnit nastavení v souboru web.config aplikace úrovně místo odebrání nastavení z konfiguračního souboru, ze kterého je zděděn.

Na kartě ověřování jsou uvedeny níže. Toto je, kde budete konfigurovat nastavení členství. Forms nastavení ověřování, zprostředkovatelů členství a role zprostředkovatele zde mohou být konfigurovány.


![](membership/_static/image4.jpg)

**Obrázek 4**


## <a name="implementing-membership-in-your-application"></a>Implementace členství v aplikaci

Nejjednodušší způsob, jak implementovat členství technologie ASP.NET 2.0 v aplikaci je pomocí zadané ovládacích prvků přihlášení. Tato metoda umožňuje implementovat základní informace o členství technologie ASP.NET 2.0 bez vůbec psaní jakéhokoli kódu.

Přihlášení ovládací prvky jsou k dispozici v technologii ASP.NET 2.0:

## <a name="login-control"></a>Ovládací prvek přihlášení

Ovládací prvek pro přihlášení poskytuje rozhraní pro uživatele k přihlášení do systému členství. Poskytne vám textboxt uživatelské jméno a heslo a přihlašovací tlačítko. Řadu dalších běžných funkcí jako odkaz na registraci pro osoby, které to ještě neudělali, zaškrtávací políčko, který umožňuje uživateli přihlášení automaticky při dalších návštěvách, odkaz pro připomenutí hesla atd. Všechny funkce řízení přihlášení lze přizpůsobit prostřednictvím vlastnosti ovládacího prvku.

V technologii ASP.NET 1.x, vývojáři měli zapsat správného množství kódu uděláte vyhledávací při použití ověřování pomocí formulářů. S členství technologie ASP.NET 2.0 můžete ověřit uživatele bez vůbec psaní jakéhokoli kódu. ASP.NET automaticky provede hledání uživatele. (Pokud používáte ovládací prvek pro přihlášení bez použití členství technologie ASP.NET, můžete použít **OnAuthenticate** metodu pro ověření uživatele.)

## <a name="loginview-control"></a>Ovládací prvek LoginView

Ovládací prvek LoginView je šablonované ovládací prvek, který poskytuje dvě šablony ve výchozím nastavení; AnonymousTemplate a LoggedInTemplate. Určuje šablonu, která se zobrazí, jestli je uživatel přihlášen do systému členství. Tento ovládací prvek se obvykle používá k zobrazení ovládacího prvku přihlášení, když uživatel není přihlášen ještě a ovládací prvek LoginStatus nebo jiných ovládacích prvků pro přihlášení, když je uživatel přihlášen. Pokud používáte správu role v aplikaci ASP.NET, můžete zobrazit ovládací prvek LoginView konkrétní šablonu na základě role uživatele. (Další technologie ASP.NET správa rolí se budeme později.)

## <a name="passwordrecovery-control"></a>Ovládací prvek PasswordRecovery

Ovládací prvek PasswordRecovery umožňuje uživatelům přijímat e-mail s své aktuální heslo nebo resetovat své heslo. Prostý text a šifrované hesla můžete obnovit a e-mailem uživatelům. Pokud je použita hodnota hash hesla, nelze obnovit. Uživatel místo toho bude nutné provést resetování hesla.

## <a name="loginstatus-control"></a>Ovládací prvek LoginStatus

Ovládací prvek LoginStatus slouží k zobrazení na ukazatel přihlášení pro uživatele, kteří nejsou protokolovány v a na ukazatel odhlášení uživatelům, kteří jsou aktuálně přihlášeni. Vlastnost Request.IsAuthenticated slouží k určení které indikátor a zobrazit. Ukazatel zobrazí LoginStatus řízení může být text (implementuje prostřednictvím **LoginText** a **LogoutText** vlastnosti) nebo bitové kopie (implementuje prostřednictvím **LoginImageUrl**a **LogoutImageUrl** vlastnosti.)

Když se uživatel přihlásí pomocí ovládacího prvku LoginStatus, uživatel se přesměruje na adresu URL zadanou v **LogoutPageUrl** vlastnost. Pokud tuto vlastnost není nastavena, je aktualizovat aktuální stránce. Vzhledem k tomu, že webu je pravděpodobně chráněn ověřování pomocí formulářů, aktualizace aktuální stránky přesměruje uživatele na přihlašovací stránku pro tuto lokalitu.

## <a name="loginname-control"></a>Ovládací prvek LoginName

Ovládací prvek LoginName zobrazí uživatelské jméno uživatele aktuálně přihlášen na web.

## <a name="createuserwizard-control"></a>Ovládací prvek CreateUserWizard

Ovládací prvek CreateUserWizard poskytuje uživatelům pohodlný způsob, jak zaregistrovat pro váš systém členství. Můžete přidat kroky (implementovaný jako kolekce WizardSteps) přes rozhraní vidíte níže.


![](membership/_static/image5.jpg)

**Obrázek 5**


CreateUserWizard je šablonované ovládací prvek, který je odvozena od třídy průvodce a poskytuje následujících šablon:

- **HeaderTemplate** této šablony řídí vzhled záhlaví průvodce.
- **Třída SidebarTemplate** této šablony řídí vzhled na bočním panelu Průvodce.
- **Oddíl StartNavigationTemplate** této šablony ovládací prvky vzhled navigace jsou v průvodci v kroku spuštění.
- **Oddíl StepNavigationTemplate** této šablony řídí vzhled navigační oblast, pokud není v kroku spuštění nebo dokončení.
- **Oddíl FinishNavigationTemplate** této šablony řídí vzhled navigační oblast v kroku dokončit.

Kromě toho pro každý krok, která přidáte do průvodce, ASP.NET vytvoří vlastní šablonu, která obsahuje ContentTemplate a oddíl CustomNavigationTemplate pro tento krok. Úplné podrobnosti o přizpůsobení CreateUserWizard naleznete v dokumentaci VS.NET 2005:

## <a name="changepassword-control"></a>Změna hesla byla ovládací prvek

Změna hesla byla řízení umožňuje uživatelům změnit své heslo. Pokud je vlastnost DisplayUserName pravdivá (je ve výchozím nastavení false), uživatel může změnit své heslo, když uživatel není přihlášený. Pokud uživatel *je* již přihlášení a DisplayUserName vlastnost na hodnotu true, uživatel bude moct změnit heslo jiného uživatele, který není přihlášen poskytuje, které znají ID uživatele u tohoto uživatele.

Uvědomte si, že pokud chcete uživatelům umožnit změnu hesla bez nutnosti přihlásit, musíte zajistit, že stránka, na kterém se zobrazí ovládací prvek Změna hesla byla umožňuje anonymní přístup. Samozřejmě uživatelé budou muset zadat svoje staré heslo. Chcete-li změnit své heslo.

## <a name="role-management"></a>Správa rolí

Správa rolí umožňuje uživatelům přiřadit konkrétní roli a pak omezit přístup k určité souboru nebo složky na základě této role. Správa rolí také poskytuje rozhraní API, takže můžete prostřednictvím kódu programu určit someones roli nebo zjistit všechny uživatele v konkrétní roli a reagují odpovídajícím způsobem.

Správa rolí není požadavek v členství technologie ASP.NET, ani se členství vyžaduje, aby bylo možné používat správu rolí. Ale vhodně dva vzájemně doplňují a je pravděpodobné, že vývojáři budou použita ve spojení mezi sebou.

Pokud chcete povolit správu role v aplikaci, proveďte následující změny v souboru web.config:

[!code-xml[Main](membership/samples/sample2.xml)]

Když **cacheRolesInCookie** je atribut nastaven na hodnotu true, ASP.NET ukládá do mezipaměti členství v rolích uživatelů do souboru cookie na straně klienta. To umožňuje hledání role proběhnout bez volání do Poskytovatel RoleProvider. Pokud používáte tento atribut, doporučujeme, aby zajistěte, aby se vývojáři **cookieProtection** je atribut nastaven na všechny. (Toto je výchozí nastavení). To zajišťuje, že cookie data se šifrují a pomáhá zajistit, aby nedošlo ke změně obsahu soubory cookie. Můžete přidat role pomocí nástroje pro správu webu. Umožňuje snadno definovat role, konfigurace přístupu k části webu na základě těchto rolí a přiřadit uživatele k rolím.


![](membership/_static/image6.jpg)

**Obrázek 6**


Jako v příkladu nahoře, stačí zadat název role a potom klikněte na Přidat Role můžete přidat nové role. Existující role můžete spravovat nebo odstranit kliknutím na příslušný odkaz v seznamu existující role.

Když spravujete roli, můžete přidat nebo odebrat uživatele, jak je uvedeno níže.


![](membership/_static/image7.jpg)

**Obrázek 7**


Zaškrtnutím políčka je v roli uživatele, můžete snadno přidat uživatele do určité role. ASP.NET se automaticky aktualizuje databázi členství pomocí příslušné položky. Také můžete nakonfigurovat pravidla přístupu pro vaši aplikaci. 1.x vývojáře využívající technologii ASP.NET se seznámíte s to prostřednictvím &lt;autorizace&gt; element v souboru web.config a tato možnost je stále k dispozici v technologii ASP.NET 2.0. Však jeho snazší spravovat přístup pravidla pomocí webu nástroje pro správu jak je uvedené níže.


![](membership/_static/image8.jpg)

**Obrázek 8**


V takovém případě je označený složce správy (jeho obtížné zobrazit, protože nástroj zvýrazní ji světla šedě) a rolí správce udělil přístup. Všechny ostatní uživatelé mají odepřený. Kliknutím na ikonu head, vyberte pravidlo a pak použijte tlačítka nahoru a dolů uspořádat pravidla. Stejně jako u ASP.NET &lt;autorizace&gt; elementu pravidla se zpracovávají v pořadí, ve kterém jsou zobrazeny. Jinými slovy Pokud byly obrácený pořadí pravidla v výše uvedený snímek, nikdo by mít přístup ke složce pro správu vzhledem k tomu, že první pravidlo, které by se mohl dostat ASP.NET bude pravidlo, které odepře everyone ke složce.

ASP.NET 2.0 přidá soubor web.config do složky, pro který určíte pravidlo přístupu. Pravidla přístupu se dá upravit pomocí konfiguračního souboru nebo pomocí nástroje Správa webu. Jinými slovy nástroj pro správu webu je jednoduše rozhraní, pomocí kterého lze upravit konfigurační soubor v uživatelské prostředí.

## <a name="using-roles-in-code"></a>Použití rolí v kódu

Rozhraní API pro správu rolí nezměnil od verze 1.x. **IsInRole** metoda se používá k určení, pokud je uživatel v konkrétní roli.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET také vytvoří instanci RolePrincipal jako člen aktuálního kontextu. Objekt RolePrincipal slouží k získání všech rolí, do kterých uživatel patří následujícím způsobem:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Pomocí ovládacího prvku LoginView kolekci RoleGroups

Teď, když máte představu o správu rolí a členství, umožňuje stručně popisují, jak ovládací prvek LoginView přebírá tuto funkci využít technologii ASP.NET 2.0. Jak bylo popsáno dříve je ovládací prvek LoginView ovládacího prvku podle šablony, která obsahuje dvě šablony ve výchozím nastavení; AnonymousTemplate a LoggedInTemplate. V rámci LoginView úlohy, které je odkaz (viz následující obrázek), můžete upravit kolekci RoleGroups.


![](membership/_static/image9.jpg)

**Obrázek 9**


Každý objekt RoleGroup obsahuje pole řetězce, které definuje které role, které se vztahuje RoleGroup. Pokud chcete přidat nový RoleGroup do ovládacího prvku LoginView, klikněte na odkaz Upravit kolekci RoleGroups. Na předchozím obrázku vidíte, že I přidali nové RoleGroup pro správce. Výběrem této RoleGroup (RoleGroup[0]) z rozevíracího seznamu zobrazení, I můžete nakonfigurovat šablonu, která se zobrazí pouze členům role správce. Na obrázku níže I přidali nové RoleGroup, která se vztahuje na členy organizační jednotky prodej role a role distribučního. Tento postup přidá druhý RoleGroup do zobrazení rozevíracího seznamu v dialogovém okně Úkoly LoginView a nic přidat do této šablony se bude zobrazovat jakéhokoli uživatele v organizační jednotky prodej nebo distribuce role.


![](membership/_static/image10.jpg)

**Obrázek 10**


## <a name="overriding-the-existing-membership-provider"></a>Přepsání existujícího poskytovatele členství

Existuje několik způsobů, že můžete rozšířit funkce členství technologie ASP.NET. Nejprve všech můžete změnit samozřejmě stávající funkce SqlMembershipProvider třídy, která dědí z něj a přepsáním její metody. Například pokud chcete implementovat vlastní funkce při vytváření uživatelů, můžete vytvořit vlastní třídu, která dědí z SqlMembershipProvider následujícím způsobem:

[!code-csharp[Main](membership/samples/sample5.cs)]

Pokud chcete na druhé straně vytvořit vlastního poskytovatele (Chcete-li například ukládat informace o členství v databázi aplikace Access), můžete vytvořit vlastního poskytovatele.

## <a name="creating-your-own-membership-provider"></a>Vytvoření vlastního poskytovatele členství

Chcete-li vytvořit vlastního zprostředkovatele členství, bude nejprve muset vytvořit třídu, která dědí z třídy MembershipProvider. Pokud používáte VB.NET, Visual Studio 2005 přidá zástupných procedur pro všechny metody, které je nutné přepsat. Pokud používáte C#, jeho až můžete přidat zástupných procedur.

Je nutné přepsat následující:

- Vlastnost ApplicationName
- Změna hesla byla – funkce
- ChangePasswordQuestionAndAnswer – funkce
- CreateUser – funkce
- DeleteUser – funkce
- EnablePasswordReset vlastnost
- EnablePasswordRetrieval vlastnost
- FindUsersByEmail – funkce
- FindUsersByName – funkce
- GetAllUsers – funkce
- GetNumberOfUsersOnline – funkce
- GetPassword – funkce
- GetUser – funkce
- GetUserNameByEmail – funkce
- Vlastnost MaxInvalidPasswordAttempts
- Hodnota MinRequiredNonAlphanumericCharacters vlastnost
- Vlastnost MinRequiredPasswordLength
- Vlastnost PasswordAttemptWindow
- Vlastnost PasswordFormat
- Vlastnost PasswordStrengthRegularExpression
- RequiresQuestionAndAnswer vlastnost
- RequiresUniqueEmail vlastnost
- ResetPassword – funkce
- Odemknout uživatele – funkce
- UpdateUser – funkce
- ValidateUser – funkce

Thats poměrně seznam implementovat jako vývojář C#. Možná bude jednodušší vytvořit třídu v VB.NET bez žádnou implementaci a pak pomocí rozhraní .NET Reflector nebo podobné nástroj převést kód C#.

Připojovací řetězec a další vlastnosti, musí být nastavená na výchozí nastavení v metodě inicializovat. (Metoda Initialize je aktivována, jestliže se načíst za běhu.) Metoda Initialize druhý parametr je typu System.Collections.Specialized.NameValueCollection a odkaz na &lt;přidat&gt; element, který je přidružen vlastního poskytovatele v souboru web.config. Tuto položku vypadá takto:

[!code-xml[Main](membership/samples/sample6.xml)]

Tady je příklad metoda Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Aby bylo možné ověření uživatele při odeslání přihlašovací formulář, musíte použít metodu ValidateUser. Tato metoda se aktivuje, když uživatel klikne na tlačítko přihlášení v ovládacím prvku přihlášení. Umístíte kód, který nemá vyhledávání uživatele v této metodě.

Jak vidíte, zápis vlastního zprostředkovatele členství není obtížné a umožňuje rozšířit tento výkonné funkce technologie ASP.NET 2.0.
