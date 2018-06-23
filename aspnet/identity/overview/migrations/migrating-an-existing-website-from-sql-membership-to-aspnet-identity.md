---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrace existující web z členství SQL na identitě ASP.NET Identity | Microsoft Docs
author: Rick-Anderson
description: Tento kurz ukazuje kroky při migraci stávající webovou aplikaci s uživatelů a data role, které jsou vytvořené pomocí SQL členství na novou identitu ASP.NET s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 1766c11dabec3931ec2bfc4ae2e15332427d7855
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314010"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrace existující web z členství SQL na identitě ASP.NET Identity
====================
podle [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Tento kurz ukazuje kroky při migraci stávající webovou aplikaci s uživatelů a data role, které jsou vytvořené pomocí členství SQL do nového systému ASP.NET Identity. Tento postup zahrnuje změnu existující schéma databáze na jednu potřeba pro ASP.NET Identity a háku ve staré nebo nové třídy do ní. Po tento přístup přijmout, po migraci databáze, budou budoucí aktualizace Identity pak moci bez obtíží zpracovávat.


V tomto kurzu jsme bude trvat šablony webové aplikace (webových formulářů) vytvořený pomocí sady Visual Studio 2010 pro vytvoření data uživatele a role. Skripty SQL použijeme pak migrovat existující databázi do tabulky, které jsou potřebné pro systém identit. Potom jsme budete nainstaluje potřebné balíčky NuGet a přidat nové stránky účtu správy, které používají systém identit pro správu členství. Jako test migrace uživatelé vytvořené pomocí SQL členství by měli být schopni se přihlásit a noví uživatelé musí být možné zaregistrovat. Můžete najít je kompletní ukázka [zde](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Viz také [migrace z členství technologie ASP.NET na ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Začínáme

### <a name="creating-an-application-with-sql-membership"></a>Vytváření aplikací s členstvím SQL

1. Je potřeba spustit s existující aplikace, která používá SQL členství a obsahuje data uživatele a role. Pro účely tohoto článku vytvoříme webovou aplikaci v sadě Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Pomocí nástroje Konfigurace ASP.NET vytvořte uživatele 2: **oldAdminUser** a **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Umožňuje vytvořit roli s názvem správce a přidejte 'oldAdminUser' jako uživatel v dané roli.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Vytvořte oddíl správce lokality, se Default.aspx. Nastavte značku autorizace v souboru web.config povolit přístup pouze uživatelům v role správce. Další informace naleznete zde [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Zobrazte v Průzkumníku serveru pochopit tabulek vytvořené systém členství SQL databázi. Data přihlášení uživatele je uložena v aspnet\_uživatelů a aspnet\_tabulky členství, zatímco role data jsou uložena v aspnet\_role tabulky. Informace o tom, které jsou uživatelé v rolích, které je uložen v aspnet\_UsersInRoles tabulky. Pro správu základní členství je dostačující k portu informace v předchozích tabulkách v identitě ASP.NET Identity systému.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrace na Visual Studio 2013

1. Instalace Visual Studio Express 2013 pro Web nebo Visual Studio 2013 spolu s [nejnovější aktualizace](https://www.microsoft.com/download/details.aspx?id=44921).
2. Otevřete projekt výše v nainstalované verzi sady Visual Studio. Pokud na počítači není nainstalována SQL Server Express, se zobrazí výzva, otevřete projekt, protože používá připojovací řetězec SQL Express. Buď můžete nainstalovat SQL Express nebo jako obejít změnit připojovací řetězec na instanci LocalDb. V tomto článku jsme budete ho změnit na instanci LocalDb.
3. Otevřete soubor web.config a změnit připojovací řetězec z. SQLExpess k v11.0 (LocalDb). Odebrat ' uživatelské Instance = true "z připojovacího řetězce.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Otevřete Průzkumníka serveru a ověřte, že může být dodržen schématu tabulky a data.
5. Systém identit ASP.NET pracuje s verze 4.5 nebo vyšší rozhraní Framework. Změňte cíl aplikací na 4.5 nebo vyšší.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Sestavte projekt a ověřte, zda nejsou žádné chyby.

### <a name="installing-the-nuget-packages"></a>Instalace balíčků Nuget

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt &gt; **spravovat balíčky NuGet**. Do vyhledávacího pole zadejte "Asp.net Identity". Vyberte balíček, v seznamu výsledků a kliknutím na tlačítko Instalovat. Přijměte licenční smlouvu kliknutím na tlačítko "I přijmout". Všimněte si, že tento balíček nainstaluje balíčky závislost: EntityFramework a Microsoft ASP.NET Identity Core. Podobně nainstalujte následující balíčky (Přeskočit poslední 4 balíčky OWIN, pokud nechcete povolit přihlášení OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrace databáze do nového systému Identity

Dalším krokem je migrovat existující databázi do schématu vyžaduje systém ASP.NET Identity. K dosažení to spustíme SQL skriptu, který má sadu příkazů pro vytvoření nové tabulky a migrovat stávající informace o uživateli do nové tabulky. Nelze nalézt soubor skriptu [zde](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Tento soubor skriptu je specifická pro tuto ukázku. Pokud schéma pro tabulky vytvořili pomocí SQL členství upravit nebo změnit skripty potřeba změnit odpovídajícím způsobem.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Jak vygenerovat skript SQL pro migraci schématu

Pro ASP.NET Identity třídy pracovat předinstalované s daty stávajících uživatelů je potřeba migrovat schéma databáze do jedné vyžaduje ASP.NET Identity. Můžeme provést přidáním nové tabulky a kopírování stávající informace o do těchto tabulek. Ve výchozím nastavení používá ASP.NET Identity EntityFramework k mapování tříd modelu Identity zpět do databáze pro ukládání a načítání informací. Tyto třídy modelu implementovat rozhraní Identity základní definice uživatele a objekty role. Tabulky a sloupce v databázi jsou založené na těchto třídách modelu. Třídy modelu objektu EntityFramework Identity v2.1.0 a jejich vlastnosti jsou definovány níže

| **IdentityUser** | **Typ** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| ID | odkazy řetězců | ID | RoleId | ProviderKey | ID |
| Uživatelské jméno | odkazy řetězců | Název | ID uživatele | ID uživatele | Typ ClaimType |
| PasswordHash | odkazy řetězců |  |  | LoginProvider | ClaimValue |
| SecurityStamp | odkazy řetězců |  |  |  | Uživatel\_Id |
| E-mailu | odkazy řetězců |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Telefonní číslo | odkazy řetězců |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Je potřeba mít tabulky se sloupci odpovídající vlastnosti pro každou z těchto modelů. Mapování mezi třídami a tabulek je definována v `OnModelCreating` metodu `IdentityDBContext`. To se označuje jako metodu fluent API konfigurace a další informace naleznete [zde](https://msdn.microsoft.com/data/jj591617.aspx). Konfigurace pro třídy je, jak je uvedeno níže

| **– Třída** | **Tabulka** | **Primární klíč** | **Cizí klíč** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | ID |  |
| IdentityRole | AspnetRoles | ID |  |
| IdentityUserRole | AspnetUserRole | ID uživatele + RoleId | Uživatel\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | ID | Uživatel\_Id -&gt;AspnetUsers |

Tyto informace můžeme vytvořit příkazy SQL k vytvoření nové tabulky. Nemůžeme zápisu každý příkaz samostatně nebo generovat celý skript pomocí příkazů prostředí PowerShell objektu EntityFramework, které jsme pak můžete upravit podle potřeby. To uděláte, v VS otevřete **Konzola správce balíčků** z **zobrazení** nebo **nástroje** nabídky

- Spuštěním příkazu "Enable-Migrations" Povolit migrace objektu EntityFramework.
- Spusťte příkaz "Add-migration počáteční", který vytvoří počáteční instalační kód k vytvoření databáze v jazyce C# nebo VB.
- Posledním krokem je spuštění "Update-Database – skriptu" příkaz, který generuje skript SQL podle třídy modelu.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Tento skript generování databáze můžete použít jako počáteční kde jsme budete provádět další změny, které chcete přidat nové sloupce a zkopírovat data. To výhodou je, že se vygeneruje `_MigrationHistory` tabulky, který je používán EntityFramework k úpravám schématu databáze, kdy modelu třídy změnu pro budoucí verze Identity verzí. 

Informace o členství uživatele v SQL měl další vlastnosti kromě těm, které jsou ve třídě modelu Identity uživatele konkrétně e-mailu, pokusů o zadání hesla, datum posledního přihlášení, datum posledního uzamčení atd. To je užitečné informace a rádi bychom znali se přenášejí do systém identit. To můžete provést přidáním další vlastnosti modelu uživatele a jejich mapování zpět na sloupce tabulky v databázi. Jsme to můžete udělat přidání třídy této podtřídy `IdentityUser` modelu. Jsme můžete přidávat vlastnosti pro tuto vlastní třídu a upravte skript SQL se při vytváření tabulky přidat na odpovídající sloupce. Kód pro tuto třídu je popsány dále v článku. Skript SQL pro vytvoření `AspnetUsers` tabulky po přidání nových vlastností by být

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Další musíme zkopírovat informace z databáze SQL členství nově přidané tabulky pro identitu. To lze provést prostřednictvím SQL tak, že zkopírujete data přímo z jedné tabulky do jiné. Přidání dat do řádky tabulky, použijeme `INSERT INTO [Table]` vytvořit. Zkopírovat z jiné tabulky, můžeme použít `INSERT INTO` příkaz spolu s `SELECT` příkaz. Získat všechny informace o uživateli je potřeba zadat dotaz *aspnet\_uživatelé* a *aspnet\_členství* tabulky a zkopírujte data, která mají *AspNetUsers*tabulky. Používáme `INSERT INTO` a `SELECT` spolu s `JOIN` a `LEFT OUTER JOIN` příkazy. Další informace o dotazování a kopírování dat mezi tabulkami, najdete v části [to](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) odkaz. Kromě AspnetUserLogins a AspnetUserClaims tabulky jsou prázdné na začátku vzhledem k tomu, že nejsou žádné informace v členství SQL, který se mapuje na to, ve výchozím nastavení. Veškeré informace zkopírovat je pro uživatele a role. Pro projekt vytvořili v předchozím kroku bude příkaz jazyka SQL pro kopírování informace do tabulky uživatelů

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Výše uvedený příkaz SQL, informace o jednotlivých uživatelů z *aspnet\_uživatelé* a *aspnet\_členství* tabulky se zkopíruje do sloupce  *AspnetUsers* tabulky. Úpravy pouze na jednom místě je, když jsme zkopírovat heslo. Vzhledem k tomu, že algoritmus šifrování hesla v členství SQL použít 'PasswordSalt' a 'PasswordFormat', jsme zkopírujte který příliš společně s hodnoty hash hesla, aby se může použít k dešifrování hesla identita. To se vysvětluje dále v článku při zapojování hasher vlastního hesla. 

Tento soubor skriptu je specifická pro tuto ukázku. Pro aplikace, které mají další tabulky můžete provést vývojáři podobný postup přidat další vlastnosti pro třídu modelu uživatele a jejich namapování na sloupce v tabulce AspnetUsers. Pro spuštění skriptu,

1. Otevřete Průzkumníka serveru. Rozbalte položku "ApplicationServices" připojení k zobrazení tabulek. Klikněte pravým tlačítkem na uzel tabulky a vyberte možnost 'nový dotaz.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. V okně dotazu zkopírujte a vložte celý skript SQL ze souboru Migrations.sql. Spusťte soubor skriptu stiskne tlačítko se šipkou "Spustit".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aktualizujte okno Průzkumníka serveru. Pět nové tabulky vytvářejí v databázi.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Níže je, jak jsou informace v tabulky členství SQL mapované na nový systém identit.

    ASPNET\_role –&gt; AspNetRoles

    ASP\_netUsers a asp\_netMembership –&gt; AspNetUsers

    aspnet\_UserInRoles --&gt; AspNetUserRoles

    Jak je popsáno v části výše, AspNetUserClaims a AspNetUserLogins tabulky jsou prázdné. Pole 'Diskriminátoru' v tabulce AspNetUser by měl odpovídat názvu třídy modelu, která je definována jako další krok. Také PasswordHash sloupec je ve formátu ' zašifrované heslo | salt hesla | formát hesla '. To umožňuje použít speciální SQL členství kryptografických logiku, takže můžete znovu použít původní hesla. Později v článku, který je vysvětleno v.

### <a name="creating-models-and-membership-pages"></a>Vytváření modelů a stránky členství

Jak už bylo zmíněno dříve, funkci Identity Entity Framework používá ke komunikaci s databázi pro ukládání informací o účtu ve výchozím nastavení. Pro práci s existujícími daty v tabulce, potřebujeme vytvořit model třídy, které mapování zpět do tabulky a propojte je v systému Identity. V rámci Identity kontraktu třídy modelu buď musí implementovat rozhraní definované v knihovně dll Identity.Core nebo můžete rozšířit stávající provádění těchto rozhraní, které jsou k dispozici v Microsoft.AspNet.Identity.EntityFramework.

V našem ukázce tabulky AspNetRoles, AspNetUserClaims, AspNetLogins a AspNetUserRole obsahovat sloupce, které jsou podobné existující implementace systém identit. Proto jsme můžete znovu použít existující tříd pro mapování na tyto tabulky. AspNetUser tabulka obsahuje některé další sloupce, které se používají k ukládání Další informace z tabulky členství SQL. To lze mapovat tak, že vytvoříte třídu modelu, který rozšířit stávající implementace 'IdentityUser' a přidat další vlastnosti.

1. Složku modely vytvoří v projektu a přidejte třídu uživatele. Název třídy by měl odpovídat data byla přidána ve sloupci 'Diskriminátoru' tabulky 'AspnetUsers'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Třídu uživatelů by měl rozšířit třídu IdentityUser v nalezen *Microsoft.AspNet.Identity.EntityFramework* knihovny dll. Deklarujte vlastnosti ve třídě, které mapování zpět do AspNetUser sloupce. Vlastnosti ID, uživatelské jméno, PasswordHash a SecurityStamp jsou definovány v IdentityUser a tak byly vynechány. Níže je kód pro třídu uživatele, která má všechny vlastnosti

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Třídu Entity Framework DbContext požadované pro zachování dat v modelech zpět na tabulky a načtení dat z tabulky k naplnění modely. *Microsoft.AspNet.Identity.EntityFramework* dll definuje třídu IdentityDbContext, která komunikuje s tabulkami Identity k načtení a uchovávání informací. IdentityDbContext&lt;tuser&gt; trvá 'TUser' třídu, která může být jakákoli třída, která rozšiřuje třídu IdentityUser.

    Vytvořte novou třídu ApplicationDBContext, který rozšiřuje IdentityDbContext ve složce 'Modely' předávání ve třídě "Uživatelem" vytvořili v kroku 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Správa uživatelů v novém systému Identity se provádí pomocí objekt UserManager&lt;tuser&gt; třídy definované v *Microsoft.AspNet.Identity.EntityFramework* knihovny dll. Je potřeba vytvořit vlastní třídu, která rozšiřuje objekt UserManager, předávání ve třídě "Uživatelem" vytvořili v kroku 1.

    Ve složce modely vytvořte novou třídu objekt UserManager, který rozšiřuje objekt UserManager&lt;uživatele&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Hesla uživatelů aplikace jsou zašifrované a uloženy v databázi. Kryptografický algoritmus používaný v členství SQL se liší od v nové systém identit. Chcete-li znovu použít původní hesla musíme selektivně dešifrování hesla při staré uživatele přihlásit pomocí algoritmu členství SQL při použití kryptografický algoritmus pro nové uživatele v identitě.

    Třída nastavení UserManager má vlastnost 'PasswordHasher', která ukládá instance třídy, který implementuje rozhraní 'ipasswordhasher.'. To se používá k šifrování a dešifrování hesla během transakce ověřování uživatele. Ve třídě nastavení UserManager definovaný v kroku 3 a vytvořte novou třídu SQLPasswordHasher a zkopírujte níže uvedeného kódu.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Vyřešte chyby kompilace importováním System.Text a System.Security.Cryptography obory názvů.

    Metoda EncodePassword šifruje heslo podle výchozí SQL členství kryptografických implementace. Toto jsou převzaty z knihovny dll System.Web. Pokud původní aplikace používat vlastní implementaci pak by měly být zohledněny sem. Je potřeba definovat dvě jiné metody *HashPassword* a *VerifyHashedPassword* využívající *EncodePassword* metoda hash dané heslo nebo ověřit, zda jako prostý text heslo s je míra existující v databázi.

    Systém členství SQL použít PasswordHash, PasswordSalt a PasswordFormat hodnoty hash hesla uživatele zadané při registraci nebo změnit své heslo. Během migrace všech tří polí jsou uložené ve sloupci PasswordHash v tabulce AspNetUser oddělených ' |' znak. Při přihlášení uživatele a heslo, má tato pole, používáme kryptografických členství SQL ke kontrole heslo; v opačném případě používáme kryptografických výchozí systém identit ověřit heslo. Tímto způsobem staré uživatele nebude muset změnit jejich hesla po migraci aplikace.
5. Konstruktor pro třídu nastavení UserManager deklarovat a to předat jako SQLPasswordHasher vlastnost v konstruktoru.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Vytvořit nový účet správy stránky

Dalším krokem při migraci je přidat účet správy stránky, které vám umožní zaregistrovat a přihlásit uživatele. Stránky starý účet z členství SQL pomocí ovládacích prvků, které nefungují s novým systémem identit. Chcete-li přidat nového uživatele správu stránek postupovat v kurzu na tento odkaz [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) od kroku, přidání webové formuláře pro registrace uživatelů do vaší aplikace' vzhledem k tomu, že jsme už máte vytvořené projektu a přidat NuGet balíčky.

Je potřeba provést některé změny pro ukázkové pro práci s projekt, který bychom měli sem.

- Kód Register.aspx.cs a Login.aspx.cs za použití třídy `UserManager` z balíčků Identity pro vytvoření uživatele. V tomto příkladu použijte objekt UserManager přidat ve složce modely podle kroků uvedených výše.
- Použijte třídu uživatele vytvořit místo IdentityUser v Register.aspx.cs a Login.aspx.cs kódu třídy. To zachytí v Naše třída vlastní uživatele do systému Identity.
- Část k vytvoření databáze mohou být přeskočeny.
- Třeba nastavit ApplicationId pro nového uživatele tak, aby odpovídala aktuální ID aplikace. To můžete provést před vytvořením objektu uživatele ve třídě Register.aspx.cs dotazování ApplicationId pro tuto aplikaci a jeho nastavení před vytvořením uživatele. 

    Příklad:

    Zadejte metodu Register.aspx.cs stránce k dotazování aspnet\_aplikace tabulky a získat Id aplikace podle názvu aplikace

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Nyní získat tuto možnost nastavíte na objekt uživatele

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Použijte původní uživatelské jméno a heslo k přihlášení stávajícího uživatele. Na stránce registrace můžete vytvořit nového uživatele. Také ověřte, že uživatelé jsou v rolích podle očekávání.

Portování do systém identit pomáhá uživateli přidat Open Authentication (OAuth) k aplikaci. Naleznete v ukázce [sem](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) jehož OAuth povolen.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jsme vám ukázal, jak k portu uživatelům z SQL členství ASP.NET Identity, ale nemůžeme nebyla data profilu portu. V dalším kurzu budete podíváme do portování data profilu z členství SQL do nového systému Identity.

Můžete ponechat zpětná vazba v dolní části tohoto článku.

*Díky tní Dykstra a Rick Anderson kontroly článek.*
