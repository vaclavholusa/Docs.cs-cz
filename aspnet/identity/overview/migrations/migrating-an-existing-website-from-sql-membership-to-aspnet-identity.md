---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrace stávajícího webu z členství SQL na ASP.NET Identity | Dokumentace Microsoftu
author: Rick-Anderson
description: Tento kurz ukazuje kroky při migraci stávající webovou aplikaci s uživateli a data role vytvořené pomocí členství SQL na nový ASP.NET Identity s...
ms.author: riande
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 393d14799973e9126379743f63f79a7131206f38
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577610"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrace stávajícího webu z členství SQL na ASP.NET Identity
====================
podle [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> Tento kurz ukazuje kroky při migraci stávající webovou aplikaci s uživateli a data role vytvořené pomocí členství SQL na nový systém ASP.NET Identity. Tento postup zahrnuje změnu existující schéma databáze do jednoho, který je potřeba pro ASP.NET Identity a volání v staré a nové třídy do ní. Jakmile použijete tento přístup, až provedete migraci vaší databáze, se zpracuje bez námahy budoucí aktualizace Identity.


Pro účely tohoto kurzu my podnikneme šablony webové aplikace (webové formuláře) vytvořené pomocí sady Visual Studio 2010 uživatele a roli data vytváří. Skripty SQL použije pak migrovat existující databázi do tabulky, které jsou potřebné pro systém Identity. V další části nainstalují potřebné balíčky NuGet a přidat nové stránky správy účtu, které používají systém identit pro správu členství. Jako test migrace uživatelé vytvořené pomocí členství SQL by měli být schopni se přihlásit a noví uživatelé by se moci zaregistrovat. Úplnou ukázku najdete [tady](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Viz také [migrace z členství technologie ASP.NET na ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Začínáme

### <a name="creating-an-application-with-sql-membership"></a>Vytvoření aplikace s členství SQL

1. Musíme začít s existující aplikaci, která používá SQL členství a obsahuje data uživatelů a rolí. Pro účely tohoto článku Pojďme vytvořit webovou aplikaci v sadě Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Pomocí nástroje Konfigurace technologie ASP.NET vytvořit uživatele 2: **oldAdminUser** a **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Umožňuje vytvořit roli s názvem správce a přidejte "oldAdminUser" jako uživatel v této roli.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Vytvořte oddíl správce serveru se Default.aspx. Nastavte značku autorizace v souboru web.config pro povolení přístupu jenom na uživatele ve správcovských rolích. Další informace najdete tady [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Zobrazí se databáze v Průzkumníku serveru pochopit tabulky vytvořené v systému členství SQL. Data přihlášení uživatele je uložená v aspnet\_uživatelů a aspnet\_tabulky členství, zatímco role data se ukládají do aspnet\_role tabulky. Informace o tom, které jsou uživatelé, v jaké role je uložena v aspnet\_UsersInRoles tabulky. Pro správu základní členství je dostačující k portu informace ve výše uvedené tabulky pro systém identit technologie ASP.NET.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrace na Visual Studio 2013

1. Nainstalovat Visual Studio Express 2013 for Web nebo Visual Studio 2013 spolu s [nejnovější aktualizace](https://www.microsoft.com/download/details.aspx?id=44921).
2. Otevřete výše uvedené projekt ve vaší nainstalované verzi sady Visual Studio. Pokud na počítači není nainstalován systém SQL Server Express, zobrazí se výzva při otevření projektu, protože používá připojovací řetězec SQL Express. Můžete buď jako obejít změnit připojovací řetězec na instanci LocalDb nebo nainstalovat SQL Express. Pro účely tohoto článku Změníme ji na instanci LocalDb.
3. Otevřete soubor web.config a změňte připojovací řetězec z. SQLExpess k (LocalDb) v11.0. Odebrat "User Instance = true" z připojovacího řetězce.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Otevřete Průzkumníka serveru a ověřte, že může být dodržen schéma tabulky a data.
5. Systém identit technologie ASP.NET pracuje s verze 4.5 nebo novější rozhraní Framework. Změnit cílení aplikace na 4.5 nebo vyšší.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Sestavte projekt a ověřte, že neexistují žádné chyby.

### <a name="installing-the-nuget-packages"></a>Instalace balíčků Nuget

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt &gt; **spravovat balíčky NuGet**. Do vyhledávacího pole zadejte "Asp.net Identity". Vyberte balíček, v seznamu výsledků a klikněte na nainstalovat. Přijměte licenční smlouvu po kliknutí na tlačítko "Souhlasím". Všimněte si, že tento balíček nainstaluje balíčky závislostí: EntityFramework a Microsoft, ASP.NET Core Identity. Podobně nainstalujte následující balíčky (Přeskočit poslední 4 balíčky OWIN, pokud nechcete povolit přihlášení OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrace databáze do nového systému Identity

Dalším krokem je migrovat existující databázi do schématu vyžadují systém identit technologie ASP.NET. K dosažení to spustíme SQL skriptu obsahující sadu příkazů pro vytvoření nových tabulek a migrovat stávající informace o uživateli do nové tabulky. Soubor skriptu najdete [tady](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Tento soubor skriptu je specifické pro tuto ukázku. Pokud schéma pro tabulky vytvořené pomocí členství SQL je přizpůsobit nebo skripty potřeba změnit odpovídajícím způsobem upravit.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Jak vygenerovat skript SQL pro migrace schématu

ASP.NET Identity třídy fungovat ihned s daty ze stávajících uživatelů musíme migraci schématu databáze na ten vyžadované ASP.NET Identity. Můžeme to udělat přidáním nové tabulky a kopírování stávající informace o těchto tabulkách. Ve výchozím nastavení používá ASP.NET Identity objektu EntityFramework k mapování tříd modelu Identity zpět do databáze se uloží nebo načtou informace. Tyto třídy modelu implementovat rozhraní core Identity, definování uživatele a objekty role. Tabulky a sloupce v databázi jsou založeny na tyto třídy modelu. Třídy modelu objektu EntityFramework Identity v2.1.0 a jejich vlastnosti jsou definované níže

| **IdentityUser** | **Typ** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| ID | odkazy řetězců | ID | RoleId | ProviderKey | ID |
| uživatelské jméno | odkazy řetězců | Název | ID uživatele | ID uživatele | Typ ClaimType |
| PasswordHash | odkazy řetězců |  |  | LoginProvider | ClaimValue |
| SecurityStamp | odkazy řetězců |  |  |  | Uživatel\_Id |
| E-mailu | odkazy řetězců |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Telefonní číslo | odkazy řetězců |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Potřebujeme mít tabulky se sloupci odpovídající vlastnosti pro každou z těchto modelů. Mapování mezi třídami a tabulky je definován v `OnModelCreating` metodu `IdentityDBContext`. To se označuje jako metodu rozhraní API fluent konfigurace a další informace lze nalézt [tady](https://msdn.microsoft.com/data/jj591617.aspx). Konfigurace pro třídy je, jak je uvedeno níže

| **Třída** | **Tabulka** | **Primární klíč** | **Cizí klíč** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | ID |  |
| IdentityRole | AspnetRoles | ID |  |
| IdentityUserRole | AspnetUserRole | ID uživatele a RoleId | Uživatel\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | ID uživatele -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | ID | Uživatel\_Id -&gt;AspnetUsers |

Pomocí těchto informací můžeme vytvořit příkazy SQL k vytvoření nových tabulek. Můžeme zápisu každý příkaz samostatně nebo generovat celý skript pomocí příkazů Powershellu objektu EntityFramework, které jsme pak můžete upravit podle potřeby. K tomu ve VS otevřít **Konzola správce balíčků** z **zobrazení** nebo **nástroje** nabídky

- Spuštěním příkazu "Enable-migrace" povolení migrace objektu EntityFramework.
- Spusťte příkaz "Přidat migrace počáteční", které vytvoří počáteční nastavení kód k vytvoření databáze v jazyce C# / VB.
- Posledním krokem je spuštění "aktualizace databáze – skriptu" u tříd modelu na základě příkazu, který generuje skript SQL.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Tento skript generování databáze může sloužit jako start, kde jsme budete provádět další změny přidávat nové sloupce a zkopírovat data. Výhodou je, že se vygeneruje `_MigrationHistory` tabulku, která používají EntityFramework k úpravě databázového schématu při změna v budoucích verzích Identity verze třídy modelu. 

Informace o členství uživatele SQL má jiné vlastnosti společně s těmi v třídě modelu uživatelského Identity totiž e-mailu, pokusů o zadání hesla, datum posledního přihlášení, datum posledního uzamčení atd. To je užitečné informace, a rádi bychom to být přeneseny do systému identit. To lze provést přidáním dalších vlastností do uživatelského modelu a mapování zpět na sloupce tabulky v databázi. Můžeme to udělat tak, že přidáte třídu, která je podtřídou `IdentityUser` modelu. Můžete do této vlastní třídy přidat vlastnosti a upravte skript SQL pro přidání odpovídající sloupce, při vytváření tabulky. Kód pro tuto třídu je popsáno dále v tomto článku. Skript SQL pro vytvoření `AspnetUsers` tabulky po přidání nových vlastností by

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Dále jsme muset zkopírovat stávající informace o ze služby SQL database členství nově přidané tabulky pro identitu. To můžete udělat prostřednictvím SQL tak, že zkopírujete data přímo z jedné tabulky do druhé. K přidání dat do řádky tabulky, použijeme `INSERT INTO [Table]` vytvořit. Zkopírujte z jiné tabulky, můžeme použít `INSERT INTO` příkaz spolu s `SELECT` příkazu. Chcete-li získat všechny informace o uživateli budeme muset dotázat *aspnet\_uživatelé* a *aspnet\_členství* tabulky a zkopírovat data do *AspNetUsers*tabulky. Používáme `INSERT INTO` a `SELECT` spolu s `JOIN` a `LEFT OUTER JOIN` příkazy. Další informace o dotazování a kopírování dat mezi tabulkami [to](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) odkaz. Kromě AspnetUserLogins a AspnetUserClaims tabulky jsou prázdné začneme od nejsou k dispozici žádné informace v členství SQL, který se mapuje na to ve výchozím nastavení. Jediná informace, zkopírovat je pro uživatele a role. Pro projekt vytvořený v předchozích kroků by byl dotaz SQL pro kopírování informací do tabulky uživatelů

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

U výše uvedeného příkazu jazyka SQL, informace o jednotlivých uživatelů z *aspnet\_uživatelé* a *aspnet\_členství* tabulek zkopírována do sloupce  *AspnetUsers* tabulky. Pouze změny na jednom místě se při kopírování heslo. Vzhledem k tomu použije algoritmus šifrování hesel v členství SQL "PasswordSalt" a "PasswordFormat", kopírování, která příliš spolu s hodnoty hash hesla tak, aby ho můžete použít k dešifrování hesla podle Identity. To je vysvětleno dále v tomto článku při zapojování hasher vlastní heslo. 

Tento soubor skriptu je specifické pro tuto ukázku. Pro aplikace, které mají další tabulky vývojáři postup podobný přístup se přidat další vlastnosti ve třídě model uživatele a jejich namapování na sloupce v tabulce AspnetUsers. Ke spuštění skriptu

1. Otevřete Průzkumníka serveru. Rozbalte "ApplicationServices" připojení k zobrazení tabulek. Klikněte pravým tlačítkem na uzel tabulky a vyberte možnost "nový dotaz.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. V okně dotazu zkopírujte a vložte celý skript SQL ze souboru Migrations.sql. Spuštění souboru skriptu tím, že kliknete na tlačítko se šipkou "Spustit".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aktualizujte okno Průzkumníka serveru. Pět nových tabulek jsou vytvořeny v databázi.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Níže je zpřístupněných informace v tabulce členství SQL na nový systém identit.

    ASPNET\_rolí –&gt; AspNetRoles

    ASP\_netUsers a asp\_netMembership--&gt; AspNetUsers

    aspnet\_UserInRoles --&gt; AspNetUserRoles

    Jak jsme vysvětlili výše v části, jsou prázdné tabulky AspNetUserClaims a AspNetUserLogins. Pole 'Diskriminátor' v tabulce AspNetUser by měl odpovídat názvu třídy modelu, který je definován jako další krok. Také PasswordHash sloupec je ve formě "šifrované heslo | hodnotu salt hesla | formát hesla". To umožňuje použít zvláštní logiku kryptografických členství SQL, takže můžete znovu použít staré heslo. V, který je vysvětlen později v tomto článku.

### <a name="creating-models-and-membership-pages"></a>Vytváření modelů a stránky členství

Jak už bylo zmíněno dříve, funkce Identity používá Entity Framework komunikovat s databází pro ukládání informací o účtu ve výchozím nastavení. Pro práci s existujícími daty v tabulce, potřebujeme vytvořit model tříd, které mapují zpět do tabulek a připojení je v systému identit. Jako součást smlouvy Identity tříd modelu by měla být buď implementovat rozhraní definované v knihovně dll Identity.Core nebo můžete rozšířit stávající implementaci těchto rozhraní, které jsou k dispozici v Microsoft.AspNet.Identity.EntityFramework.

V naší ukázce AspNetRoles, AspNetUserClaims, AspNetLogins a AspNetUserRole tabulky obsahují sloupce, které jsou podobné stávající implementaci systému identit. Proto jsme můžete znovu použít existující třídy pro mapování na těchto tabulkách. Tabulka AspNetUser má některé další sloupce, které se používají k ukládání dalších informací z tabulky členství SQL. To lze mapovat tak, že vytvoříte třídu modelu, který rozšířit stávající implementaci "IdentityUser" a přidejte další vlastnosti.

1. Vytvoří modely složky projektu a přidejte třídu uživatele. Název třídy by měl odpovídat data přidaná ve sloupci 'Diskriminátor' tabulky "AspnetUsers".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Třídu uživatelů by měl rozšířit třídu IdentityUser součástí *Microsoft.AspNet.Identity.EntityFramework* knihovny dll. Deklarace vlastnosti ve třídě, které mapují AspNetUser sloupce. Vlastnosti ID, uživatelské jméno, PasswordHash a SecurityStamp jsou definovány v IdentityUser a proto jsou vynechány. Níže je kód pro třídu uživatelů, který má všechny vlastnosti

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Třídu DbContext v Entity Framework je nutná k uchování dat v modelech zpět do tabulek a načtěte data z tabulek k naplnění modely. *Microsoft.AspNet.Identity.EntityFramework* knihovny dll definuje třídu IdentityDbContext, která komunikuje s tabulkami Identity k získávání a ukládání informací. IdentityDbContext&lt;tuser&gt; trvá "TUser" třídu, která může být libovolná třída, která rozšiřuje třídu IdentityUser.

    Vytvořte novou třídu ApplicationDBContext, která rozšiřuje IdentityDbContext ve složce "Modely" předávání ve třídě 'Uživatele' vytvořili v kroku 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Správa uživatelů v novém systému identit se provádí pomocí objektu UserManager.&lt;tuser&gt; třídy definované v *Microsoft.AspNet.Identity.EntityFramework* knihovny dll. Potřebujeme vytvořit vlastní třídu, která rozšiřuje objektu UserManager, předávání ve třídě 'Uživatele' vytvořili v kroku 1.

    Ve složce modely vytvořte novou třídu objektu UserManager, která rozšiřuje objektu UserManager&lt;uživatele&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Hesla uživatelů aplikace jsou zašifrované a uložené v databázi. Šifrovací algoritmus použitý v členství SQL je odlišné od těch v novém systému identit. Chcete-li znovu použít staré heslo potřebujeme selektivně dešifrování hesla staré uživatelům přihlášení pomocí algoritmu členství SQL při použití šifrovacího algoritmu pro nové uživatele v identitě.

    Třída objektu UserManager. má vlastnost 'PasswordHasher", která ukládá instance třídy, která implementuje rozhraní 'ipasswordhasher.'. Slouží k šifrování/dešifrování hesla během uživatelské transakce v ověřování. Ve třídě objektu UserManager definovaný v kroku 3, vytvořte novou třídu SQLPasswordHasher a zkopírujte níže uvedeného kódu.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Import oborech názvů System.Text slouží a System.Security.Cryptography vyřešte chyby kompilace.

    Metoda EncodePassword šifruje heslo podle implementace kryptografických členství SQL výchozí. To je převzata z knihovny dll System.Web. Pokud původní aplikace používali vlastní implementaci by měl být projeví tady. Budeme muset definovat dvě další metody *HashPassword* a *VerifyHashedPassword* , které používají *EncodePassword* metoda hash dané heslo nebo ověřit prostý text heslo s jednu existující v databázi.

    Systém členství SQL použít PasswordHash, PasswordSalt a PasswordFormat hodnoty hash hesla uživatele zadané při registraci nebo změnit své heslo. Během migrace jsou tři pole jsou uloženy v PasswordHash sloupec v tabulce AspNetUser oddělené "|" znak. Při přihlášení uživatele a heslo má tato pole, můžeme pomocí crypto členství SQL Zkontrolujte heslo. v opačném případě používáme výchozí crypto systém identit Ověřte heslo. Tímto způsobem staré uživatele nemusí ke změně hesla po migraci oznámení o aplikaci.
5. Deklarujte konstruktor pro třídu objektu UserManager a předat ji jako SQLPasswordHasher vlastnosti v konstruktoru.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Vytvořit nový účet management stránky

Dalším krokem při migraci je přidat účet správu stránky, které vám umožní uživateli zaregistrovat a přihlaste se. Starý účet stránky z členství SQL používat ovládací prvky, které nefungují s novým systémem identit. Přidání nového uživatele správu stránky, postupujte podle kurzu na tento odkaz [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) od kroku 'Přidat webové formuláře pro registrace uživatelů do vaší aplikace' protože jsme už vytvořili projektu a přidání balíčku NuGet balíčky.

Potřebujeme udělat nějaké změny ukázka fungovala s projektem, který máme k dispozici zde.

- Register.aspx.cs a Login.aspx.cs kód za použití třídy `UserManager` z balíčků identit k vytvoření uživatele. V tomto příkladu pomocí objektu UserManager přidáno ve složce modelů pomocí kroků uvedených výše.
- Použití třídy uživatel vytvořil, a nikoli IdentityUser v Register.aspx.cs a Login.aspx.cs kódu na pozadí třídy. To zavěšení ve třídě naše vlastní uživatelské do systému identit.
- Část k vytvoření databáze mohly být přeskočeny.
- Vývojář musí nastavit ApplicationId pro nového uživatele tak, aby odpovídaly aktuálním ID aplikace. To můžete udělat předtím, než je vytvořen objekt uživatele ve třídě Register.aspx.cs dotazování ApplicationId pro tuto aplikaci a nastavíte ho před vytvořením uživatele. 

    Příklad:

    Definování metody ve stránce Register.aspx.cs k dotazování aspnet\_aplikace tabulky a získání Id aplikace podle názvu aplikace

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Teď získáte nastavením této hodnoty na objekt uživatele

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Pomocí staré uživatelské jméno a heslo k přihlášení stávajícího uživatele. Na stránce registrace k vytvoření nového uživatele. Dál ověřte, že uživatelé jsou v rolích podle očekávání.

Přenos aplikací do systému identit pomáhá uživateli přidat do aplikace Open Authentication (OAuth). Najdete ukázky [tady](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) který má povolené ověřování OAuth.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jsme vám ukázali jak přenést uživatele z členství SQL na ASP.NET Identity, ale neměli jsme port data profilu. V dalším kurzu se podíváme na přenos dat profilu z členství SQL na nový systém identit.

Zpětná vazba v dolní části tohoto článku můžete nechat.

*Děkujeme, že Tomáš Dykstra a Rick Anderson revize článku.*
