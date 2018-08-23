---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Členství | Dokumentace Microsoftu
author: microsoft
description: Členství technologie ASP.NET je založena na úspěšném modelu ověřování formulářů z ASP.NET 1.x. Ověřování pomocí formulářů technologie ASP.NET poskytuje pohodlný způsob, jak incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: d7fa3cb61608ea089141931cb9362359cdc92619
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752922"
---
<a name="membership"></a>Členství
====================
podle [Microsoft](https://github.com/microsoft)

> Členství technologie ASP.NET je založena na úspěšném modelu ověřování formulářů z ASP.NET 1.x. Ověřování pomocí formulářů technologie ASP.NET poskytuje pohodlný způsob, jak začlenit přihlašovací formulář do aplikace ASP.NET a ověření proti databázi nebo jiného úložiště dat uživatelů.


Členství technologie ASP.NET je založena na úspěšném modelu ověřování formulářů z ASP.NET 1.x. Ověřování pomocí formulářů technologie ASP.NET poskytuje pohodlný způsob, jak začlenit přihlašovací formulář do aplikace ASP.NET a ověření proti databázi nebo jiného úložiště dat uživatelů. Členy třídy FormsAuthentication umožňují zpracovat soubory cookie pro ověřování, zkontrolujte platného přihlášení, přihlášení uživatele si atd. Implementace ověřování pomocí formulářů v aplikaci ASP.NET 1.x však může vyžadovat množství kódu.

Členství v technologii ASP.NET 2.0 je hlavní vylepšení oproti použití ověřování pomocí formulářů samostatně. (Členství je nejrobustnější, když se ověřování pomocí formulářů s velkou provázaností, ale ověřování pomocí formulářů není povinné.) Jak brzy zjistíte, můžete použít členství technologie ASP.NET a ovládacími prvky pro přihlášení v technologii ASP.NET 2.0 pro implementaci systému výkonné členství aniž byste museli psát většinu kódu vůbec.

## <a name="implementing-membership-in-aspnet-20"></a>Implementace členství v ASP.NET 2.0

Členství je implementována pomocí následujících čtyřech krocích. Uvědomte si, že existují mnoho dílčích kroků, které jsou zahrnuty i volitelná konfigurace, kterou je možné implementovat také. Tyto kroky jsou určeny pro ilustraci celkový přehled konfigurace členství.

1. Vytvoření databáze členství (Pokud SQL Server slouží jako úložiště členství.)
2. Zadejte možnosti členství v konfiguračních souborech aplikace. (Členství je standardně povolená.)
3. Určete typ členství úložiště, který chcete použít. Možnosti jsou: 

    - Microsoft SQL Server (verze 7.0 nebo novější)
    - Active Directory Store
    - Vlastního poskytovatele členství
4. Konfigurace aplikace pro ověřování pomocí formulářů ASP.NET. Ještě jednou členství je navržené tak, aby využít ověřování pomocí formulářů, ale ověřování pomocí formulářů není povinné.
5. Definujte uživatelské účty pro členství a v případě potřeby nakonfigurujte role.

## <a name="creating-the-membership-database"></a>Vytváří se databáze členství

Pokud provedete tak, že pomocí SQL Server 7.0 nebo vyšší jako úložiště členství, můžete použít aspnet\_regsql nástroj (k dispozici nejsnadněji z Visual Studio .NET 2005 příkazového řádku) konfigurace vaší databáze. Aspnet\_regsql nástroje lze použít jako nástroj příkazového řádku nebo pomocí Průvodce grafickým uživatelským rozhraním. Metoda Průvodce je nejjednodušší způsob, jak nakonfigurovat svou databázi. Pro přístup k průvodci, spusťte následující příkaz:

`aspnet_regsql W`

Po spuštění tohoto příkazu, zobrazí se Průvodce instalací serveru SQL pro ASP.NET jak je znázorněno níže.


![](membership/_static/image1.jpg)

**Obrázek 1**


Průvodce instalací serveru SQL pro ASP.NET vytvoří webový server v instanci, kterou zadáte v průvodci. Ale technologie ASP.NET použije připojovací řetězec v souboru machine.config pro připojení k vaší databázi. Ve výchozím nastavení, bude odkazovat tento připojovací řetězec do instance systému SQL Server 2005, takže pokud používáte instanci systému SQL Server 2000 nebo SQL Server 7.0, budete muset změnit připojovací řetězec v souboru machine.config. Tento připojovací řetězec může být umístěn zde:

[!code-xml[Main](membership/samples/sample1.xml)]

Bohužel Pokud nechcete změnit připojovací řetězec, ASP.NET vám neposkytne popisná chybová. Stačí si stěžovat o tom, že se, že jste ještě nevytvořili databázi bude pokračovat. V případě výše můžu upravit připojovací řetězec tak, aby odkazoval na můj místní instanci systému SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Určení konfigurace a přidání uživatelů a rolí

Dalším krokem konfigurace členství je přidání potřebné informace v souboru web.config aplikace. V technologii ASP.NET 1.x, upravte soubor web.config byl někdy obtížné z důvodu použití lowerCamelCase a nedostatečné technologie Intellisense. Visual Studio .NET 2005 jednodušší úlohy s podporou technologie Intellisense pro konfigurační soubory, ale ASP.NET 2.0 přestane ještě o krok tím, že poskytuje webové rozhraní pro úpravu konfiguračních souborů.

Webové rozhraní můžete spustit kliknutím na tlačítko Konfigurace technologie ASP.NET na panelu nástrojů Průzkumníka řešení, jak je znázorněno níže. Můžete také spustit webové rozhraní prostřednictvím automaticky otevíraná okna, které se zobrazí, jakmile jsou vloženy ovládacími prvky pro přihlášení.


![](membership/_static/image2.jpg)

**Obrázek 2**


Tím se spustí nástroj Správa webu technologie ASP.NET je uvedeno níže. Správa webu technologie ASP.NET je rozhraní čtyři karty, která umožňuje jednoduše spravovat nastavení aplikace. K dispozici jsou následující karty:

- **Domovská stránka**
- **Zabezpečení** konfigurovat uživatele, role a přístup.
- **Aplikace** nakonfigurovat nastavení aplikace.
- **Zprostředkovatel** konfigurace a testování vaší aplikace zprostředkovatele členství.

Nástroje pro správu webu umožňuje snadno vytvářet nové uživatele, vytvořit nové role a správě uživatelů a rolí. Tato možnost není k dispozici v rozhraní Windows. Rozhraní Windows můžete snadno definovat nastavení autorizace a chcete přidat, odstranit a spravovat poskytovatele, funkce, které nejsou v nástroji pro správu webu.

Spusťte rozhraní Windows, otevřete modul snap-in Internetová informační služba, klikněte pravým tlačítkem na aplikaci a zvolte možnost Vlastnosti. Klikněte na kartu technologie ASP.NET a potom klikněte na tlačítko Upravit konfiguraci. (Aplikace musí být spuštěn v rámci technologie ASP.NET 2.0 pro tlačítko Upravit konfigurace na povolit. Verze technologie ASP.NET můžete nakonfigurovat v okně technologie ASP.NET.) Jak je znázorněno níže, zobrazí se dialogové okno nastavení konfigurace technologie ASP.NET.


![](membership/_static/image3.jpg)

**Obrázek 3**


Na kartě Obecné nastavení aplikace a připojovacích řetězců jsou uvedeny. Všechna nastavení kurzívou jsou definovány v nadřazeném konfiguračním souboru (souboru machine.config nebo web.config na vyšší úrovni) a nastavení není v kurzívě z konfiguračního souboru aplikace. Pokud nastavení se přidá, odebrat nebo upravit na úrovni aplikace ASP.NET se přidat, odebrat nebo upravte nastavení v souboru web.config aplikace úrovně místo aby odebrala nastavení z konfiguračního souboru, ze kterého se dědí.

Na kartě ověřování je uveden níže. To je, ve kterém nakonfigurujete nastavení členství. Nastavení ověřování, zprostředkovateli členství, formulářů a zde mohou být konfigurovány zprostředkovatele rolí.


![](membership/_static/image4.jpg)

**Obrázek 4**


## <a name="implementing-membership-in-your-application"></a>Implementace členství ve vašich aplikacích

Nejjednodušší způsob, jak implementovat členství technologie ASP.NET 2.0 ve vaší aplikaci se má používat zadaný přihlašovacích ovládacích prvků. Tato metoda umožňuje implementovat základní informace o členství technologie ASP.NET 2.0 bez psaní kódu vůbec.

Následující ovládací prvky přihlášení jsou k dispozici v technologii ASP.NET 2.0:

## <a name="login-control"></a>Ovládací prvek Login

Ovládací prvek Login poskytuje rozhraní pro uživatele k přihlášení do systému členství. To vám poskytne textboxt uživatelské jméno a heslo a přihlašovací tlačítko. Mnoho dalších běžných funkcí jako je například odkaz na registraci pro uživatele, kteří ještě neučinili, zaškrtávací políčko, který umožňuje uživateli přihlášení automaticky při následných návštěvách, odkaz připomenutí hesla atd. Všechny funkce ovládacího prvku pro přihlášení jsou přizpůsobitelné prostřednictvím vlastnosti ovládacího prvku.

V technologii ASP.NET 1.x, vývojáři museli psát množství kódu vyhledávat při použití ověřování pomocí formulářů. Pomocí členství technologie ASP.NET 2.0 můžete ověřit uživatele bez psaní kódu vůbec. ASP.NET automaticky provede vyhledávání uživatele. (Pokud používáte ovládací prvek Login bez použití členství technologie ASP.NET, můžete použít **OnAuthenticate** metodu za účelem ověření uživatele.)

## <a name="loginview-control"></a>Ovládací prvek zobrazení přihlášení

Ovládací prvek zobrazení přihlášení je ovládací prvek bez vizuálního vzhledu, který obsahuje dvě šablony ve výchozím nastavení; AnonymousTemplate a LoggedInTemplate. Určuje, jestli se uživatel přihlásí do systému členství Určuje šablonu, která se zobrazí. Tento ovládací prvek obvykle slouží k zobrazení přihlášení ovládacího prvku, když uživatel není přihlášen ještě a ovládací prvek stavu přihlášení a/nebo jiných ovládacími prvky pro přihlášení, když je uživatel přihlášen. Pokud používáte správu rolí v aplikaci ASP.NET, ovládací prvek zobrazení přihlášení můžete zobrazit konkrétní šablonu na základě role uživatele. (Další technologie ASP.NET správu rolí se budeme později.)

## <a name="passwordrecovery-control"></a>Ovládací prvek PasswordRecovery

Ovládací prvek PasswordRecovery umožňuje uživatelům e-mail s aktuální heslo nebo resetovat své heslo. Prostý text a šifrovaná hesla můžete obnovit a e-mailem uživatelům. Pokud se po zahašování použije heslo, nelze obnovit. Uživatel bude místo požadované k provedení resetování hesla.

## <a name="loginstatus-control"></a>Ovládací prvek stavu přihlášení

Ovládací prvek stavu přihlášení se používá k zobrazení indikátor přihlášení pro uživatele, kteří nejsou přihlášeni a odhlášení indikátor uživatelům, kteří jsou aktuálně přihlášeni. Vlastnost Request.IsAuthenticated slouží k určení, které ukazatel k zobrazení. Indikátor zobrazuje ovládací prvek stavu přihlášení může být text (prostřednictvím rozhraní **LoginText** a **LogoutText** vlastnosti) nebo obrázky (prostřednictvím rozhraní **LoginImageUrl**a **LogoutImageUrl** vlastnosti.)

Když se uživatel přihlásí přes ovládací prvek stavu přihlášení, uživatel je přesměrován na adrese URL zadané hodnotou **LogoutPageUrl** vlastnost. Pokud není tato vlastnost nastavena, se aktualizují aktuální stránce. Protože lokality je pravděpodobně chráněn ověřování pomocí formulářů, obnovit aktuální stránku přesměruje uživatele na přihlašovací stránku pro web.

## <a name="loginname-control"></a>Prvek přihlašovacího jména

Prvek přihlašovacího jména zobrazí uživatelské jméno uživatele momentálně přihlášen na web.

## <a name="createuserwizard-control"></a>Prvku CreateUserWizard

Ovládacím prvku CreateUserWizard poskytuje uživatelům pohodlný způsob, jak zaregistrovat pro váš systém členství. Můžete přidat kroky (implementováno jako kolekci WizardSteps) přes rozhraní je uvedeno níže.


![](membership/_static/image5.jpg)

**Obrázek 5**


CreateUserWizard je ovládací prvek bez vizuálního vzhledu, který je odvozen od třídy průvodce a poskytuje následující šablony:

- **Parametr HeaderTemplate** Tato šablona určuje vzhled hlavičky průvodce.
- **Třída SidebarTemplate** Tato šablona řídí vzhled na bočním panelu Průvodce.
- **Oddíl StartNavigationTemplate** této šablony ovládacích prvků vzhled navigaci jsou v průvodci v kroku začátku.
- **Oddíl StepNavigationTemplate** Tato šablona řídí vzhled navigační oblasti nejsou v kroku zahájení nebo dokončení.
- **Oddíl FinishNavigationTemplate** Tato šablona řídí vzhled navigační oblasti na krok v kroku dokončení.

Kromě toho pro každý krok, který přidáte do Průvodce vytvořením ASP.NET vytvoří vlastní šablonu, která obsahuje ContentTemplate a oddíl CustomNavigationTemplate pro daný krok. Všechny podrobnosti o přizpůsobení CreateUserWizard naleznete v dokumentaci VS.NET 2005:

## <a name="changepassword-control"></a>Ovládací prvek ChangePassword

Ovládací prvek ChangePassword umožňuje uživatelům změnit své heslo. Je-li DisplayUserName vlastnost hodnotu true (je ve výchozím nastavení hodnotu false), uživatel může změnit své heslo, když uživatel není přihlášený. Pokud uživatel *je* přihlášeni a DisplayUserName vlastnost má hodnotu true, uživatel bude moct změnit heslo jiný uživatel, který není přihlášen poskytnutí uživatelského ID uživatele, které znají.

Uvědomte si, že pokud chcete, aby uživatelé mohli změnit hesla bez nutnosti přihlášení, budete muset zajistit, že na stránce, na kterém se zobrazí ovládací prvek ChangePassword umožňuje anonymní přístup. Samozřejmě uživatelé budou muset zadat svoje staré heslo, aby se změnil heslo.

## <a name="role-management"></a>Správa rolí

Správa rolí můžete uživatelům přiřadit konkrétní roli a pak omezit přístup k určité soubory a složky na základě této role. Správa rolí také poskytuje rozhraní API tak, že můžete programově určit možné role nebo určit všechny uživatele v konkrétní roli a reagují odpovídajícím způsobem.

Správa rolí není povinné v členství technologie ASP.NET, ani není členství požadavek, aby bylo možné používat správu rolí. Ale krásně dvou vzájemně doplňují a je pravděpodobné, že vývojáři se použít ve spojení mezi sebou.

Pokud chcete povolit správu rolí ve vaší aplikaci, proveďte následující změnu v souboru web.config:

[!code-xml[Main](membership/samples/sample2.xml)]

Když **cacheRolesInCookie** atribut je nastaven na hodnotu true, technologie ASP.NET ukládá do mezipaměti členství v roli uživatele do souboru cookie na straně klienta. To umožňuje vyhledávání role probíhat bez volání do Poskytovatel RoleProvider. Při použití tohoto atributu, vývojáři se doporučuje zajistit, aby **cookieProtection** atribut je nastaven na všechny. (Toto je výchozí nastavení.) Tím se zajistí, že cookie data jsou zašifrované a pomáhá zajistit, že nedošlo ke změně obsahu soubory cookie. Role je možné přidat pomocí nástroje pro správu webu. Umožňuje snadno definovat role, přístup k různým částem lokality na základě těchto rolí nakonfigurovat a přiřadit uživatele k rolím.


![](membership/_static/image6.jpg)

**Obrázek 6**


Jak uvádíme výš, je možné přidat nové role jednoduše zadávat název role, a potom klikněte na Přidat roli. Existující role dá spravovat nebo odstranit kliknutím na příslušný odkaz v seznamu existujících rolí.

Když spravujete roli, můžete přidat nebo odebrat uživatele, jak je znázorněno níže.


![](membership/_static/image7.jpg)

**Obrázek 7**


Zaškrtnutím políčka je v roli uživatele můžete snadno přidat uživatele do konkrétní role. ASP.NET bude automaticky aktualizovat vaše databáze členství odpovídající položky. Také můžete nakonfigurovat pravidla přístupu pro vaši aplikaci. 1.x vývojáře využívající technologii ASP.NET se seznámíte s tím prostřednictvím &lt;autorizace&gt; element v souboru web.config a tato možnost je stále k dispozici v technologii ASP.NET 2.0. Ale jeho usnadňují správu přístupu pravidla pomocí webového serveru nástroje pro správu jak je znázorněno níže.


![](membership/_static/image8.jpg)

**Obrázek 8**


V takovém případě je zvýrazněn složky správy (jeho obtížně vidíte, protože nástroj zvýrazní ji v světle šedá) a role správce udělil přístup. Všem ostatním uživatelům je odepřen. Klikněte na tlačítko na ikoně hlavního vyberte pravidlo a pak použijte tlačítka Přesunout nahoru a dolů uspořádat pravidla. Stejně jako u technologie ASP.NET &lt;autorizace&gt; elementu, pravidla se zpracovávají v pořadí, ve kterém jsou uvedeny. Jinými slovy Pokud byly obrácený pořadí pravidel ve výše uvedené snímek, nikdo by mít přístup ke složce pro správu vzhledem k tomu, že první pravidlo, které by se mohl dostat ASP.NET by být pravidlo, které zakazuje všem uživatelům ke složce.

ASP.NET 2.0 přidá soubor web.config na složku, pro které pravidlo přístupu zadáváte. Přístupová pravidla, která se dá upravit pomocí konfiguračního souboru nebo prostřednictvím nástroje pro správu webu. Jinými slovy nástroje pro správu webu je jednoduše rozhraní, pomocí kterého lze upravit konfigurační soubor v uživatelsky přívětivé prostředí.

## <a name="using-roles-in-code"></a>Použití rolí v kódu

Rozhraní API pro správu rolí nebyl změněn od verzi 1.x. **IsInRole** metoda se používá k určení, zda uživatel je v konkrétní roli.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET vytvoří instanci RolePrincipal také jako člen aktuálního kontextu. Objekt RolePrincipal umožňuje získat všechny role, do kterých uživatel patří následujícím způsobem:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Pomocí ovládacího prvku LoginView kolekci RoleGroups

Teď, když máte představu o správu rolí a členství, umožňuje stručně popisují, jak ovládacího prvku LoginView využívá tuto funkci v technologii ASP.NET 2.0. Jak bylo uvedeno výše ovládacího prvku LoginView je ovládací prvek bez vizuálního vzhledu, který obsahuje dvě šablony ve výchozím nastavení; AnonymousTemplate a LoggedInTemplate. V rámci prvku LoginView úlohy, které dialogové okno je odkaz (viz dole), která umožňuje upravit kolekci RoleGroups.


![](membership/_static/image9.jpg)

**Obrázek 9**


Každý objekt RoleGroup obsahuje pole řetězců, který definuje, jaké role, pro které platí RoleGroup. Chcete-li přidat nový RoleGroup do ovládacího prvku LoginView, klikněte na odkaz Upravit kolekci RoleGroups. Na obrázku výše vidíte, že po přidání nového typu RoleGroup pro správce. Výběrem tohoto typu RoleGroup (RoleGroup[0]) z rozevíracího seznamu zobrazení, můžete nakonfigurovat šablonu, která se zobrazí pouze členům role správce. Na obrázku níže jsem přidali nové RoleGroup, které platí pro členy role prodeje a roli distribučního. Tento postup přidá zobrazení rozevírací seznam v dialogovém okně Úlohy LoginView druhý RoleGroup a nic přidat do této šablony se nebude zobrazovat ji žádný uživatel ve Sales nebo distribuce role.


![](membership/_static/image10.jpg)

**Obrázek 10**


## <a name="overriding-the-existing-membership-provider"></a>Přepsání existující zprostředkovatele členství

Existuje několik způsobů, můžete rozšířit funkce členství technologie ASP.NET. Za prvé můžete změnit samozřejmě stávajících funkcí třídy SqlMembershipProvider dědění z něho a přepsáním metody. Například pokud chcete implementovat vlastní funkce při vytváření uživatelů, můžete vytvořit vlastní třídu odvozenou od SqlMembershipProvider následujícím způsobem:

[!code-csharp[Main](membership/samples/sample5.cs)]

Pokud chcete na druhé straně vytvořit vlastního zprostředkovatele (Chcete-li například ukládat informace o vašem členství v databázi aplikace Access), můžete vytvořit vlastního zprostředkovatele.

## <a name="creating-your-own-membership-provider"></a>Vytvoření vlastního zprostředkovatele členství

Vytvoření vlastního zprostředkovatele členství, je nejprve potřeba vytvořit třídu, která dědí z třídy MembershipProvider. Pokud používáte VB.NET, Visual Studio 2005 přidá zástupné procedury pro všechny metody, které je potřeba přepsat. Pokud používáte C# a jeho až k přidání zástupné procedury.

Je potřeba přepsat následující:

- Vlastnost ApplicationName
- Metodu ChangePassword – funkce
- ChangePasswordQuestionAndAnswer – funkce
- Metody CreateUser – funkce
- DeleteUser – funkce
- Vlastnost EnablePasswordReset
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
- Hodnota MinRequiredPasswordLength vlastnost
- Vlastnost PasswordAttemptWindow
- Vlastnost PasswordFormat
- Vlastnost PasswordStrengthRegularExpression
- RequiresQuestionAndAnswer vlastnost
- RequiresUniqueEmail vlastnost
- ResetPassword – funkce
- Odemknout uživatele – funkce
- UpdateUser – funkce
- ValidateUser – funkce

Thats poměrně seznam implementovat jako vývojář C#. Možná bude snadněji vytvářet třídy v VB.NET bez jakékoli implementaci a pak použít k převodu kód jazyka C# .NET Reflector nebo něco podobného.

Připojovací řetězec a dalších vlastností je třeba nastavit na jejich výchozích hodnotách v metodě inicializace. (Metoda Initialize je aktivována, když je načten v době běhu zprostředkovatele.) Druhý parametr do metody inicializace je typu System.Collections.Specialized.NameValueCollection a odkazem na &lt;přidat&gt; element, který je přidružený k vlastního poskytovatele v souboru web.config. Tuto položku vypadá takto:

[!code-xml[Main](membership/samples/sample6.xml)]

Tady je příklad metodu Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Chcete-li ověřit uživatele při odesílání přihlašovací formulář, musíte použít metodu ValidateUser. Tato metoda je vyvoláno, když uživatel klikne na tlačítko přihlášení do ovládacího prvku pro přihlášení. Umístíte kód, který nepodporuje vyhledávání uživatele v této metodě.

Jak vidíte, zápis poskytovatele členství není obtížné a umožňuje rozšířit toto výkonné funkce technologie ASP.NET 2.0.
