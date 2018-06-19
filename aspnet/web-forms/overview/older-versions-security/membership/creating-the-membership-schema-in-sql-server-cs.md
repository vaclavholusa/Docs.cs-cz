---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Vytvoření schématu členství v systému SQL Server (C#) | Microsoft Docs
author: rick-anderson
description: V tomto kurzu spustí prověřením techniky pro přidání schéma potřebné k databázi chcete-li použít SqlMembershipProvider. Následující, jsme wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 4fa0476ca8336b56340dd177f9816acbe015ef7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891790"
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>Vytvoření schématu členství v systému SQL Server (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> V tomto kurzu spustí prověřením techniky pro přidání schéma potřebné k databázi chcete-li použít SqlMembershipProvider. Následující, jsme prozkoumá klíče tabulky ve schématu a diskutovat o jejím účelu a význam. V tomto kurzu končí podívejte se na poskytovatele, kterého by měl používat rozhraní členství ověření pravosti aplikace ASP.NET.


## <a name="introduction"></a>Úvod

Předchozí dva kurzy zkontrolován použití ověřování pomocí formulářů k identifikaci návštěvníky webu. Rozhraní framework ověřování formulářů umožňuje snadno pro vývojáře pro přihlášení uživatele na web a jejich pamatovat napříč návštěv stránky prostřednictvím lístků pro ověřování. `FormsAuthentication` Třída obsahuje metody pro generování lístku a její přidání do souborů cookie návštěvníka. `FormsAuthenticationModule` Prozkoumá všechny příchozí požadavky a pro ty platné ověřovací lístek, vytvoří a přidruží `GenericPrincipal` a `FormsIdentity` objektu s aktuálním požadavkem. Ověřování pomocí formulářů je jenom mechanismus pro udělení na ověřovací lístek pro návštěvníka při přihlašování v a na následné žádosti, analýza tento lístek určit identitu uživatele. Pro webovou aplikaci pro podporu uživatelské účty potřebujeme ještě implementace úložiště uživatele a přidat další funkce a ověřte přihlašovací údaje, registrovat nové uživatele a velkého počtu dalších úloh souvisejícím s účtem uživatele.

Před aplikaci ASP.NET 2.0 vývojáři byly hák pro implementaci všechny tyto úlohy souvisejícím s účtem uživatele. Naštěstí týmem ASP.NET rozpoznána tohoto nedostatku a zavedená rozhraní členství s prostředím ASP.NET 2.0. Rozhraní framework členství je sada tříd v rozhraní .NET Framework, které poskytují programové rozhraní pro provádění úloh souvisejícím s účtem uživatele jádra. Toto rozhraní je vytvořené na [modelu poskytovatelů](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), což umožňuje vývojářům připojte přizpůsobené implementace standardizované rozhraní API.

Jak je popsáno v <a id="Tutorial1"> </a> [ *Základy zabezpečení a podporu ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) kurzu rozhraní .NET Framework se dodává s dvě předdefinované zprostředkovatele členství: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) a [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Jak již název napovídá, `SqlMembershipProvider` používá databázi systému Microsoft SQL Server jako úložiště uživatele. Chcete-li použít tento zprostředkovatel v aplikaci, je potřeba oznámit zprostředkovateli, jaké databáze pro použití jako úložiště. Jak je možné si můžete představit, `SqlMembershipProvider` očekává databáze úložiště uživatele tak, aby měl určité databázových tabulek, zobrazení a uložených procedur. Je potřeba přidat tento očekávané schéma k vybrané databázi.

V tomto kurzu spustí prověřením techniky pro přidání schéma potřebné k databázi chcete-li použít `SqlMembershipProvider`. Následující, jsme prozkoumá klíče tabulky ve schématu a diskutovat o jejím účelu a význam. V tomto kurzu končí podívejte se na poskytovatele, kterého by měl používat rozhraní členství ověření pravosti aplikace ASP.NET.

Můžeme začít!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Krok 1: Rozhodování, kam umístit úložiště uživatele

Aplikace technologie ASP.NET data se ukládají běžně v počet tabulek v databázi. Při implementaci `SqlMembershipProvider` schéma databáze jsme rozhodnout, zda umístěte schéma členství ve stejné databázi jako data aplikací, nebo do alternativní databáze.

I doporučujeme vyhledání schéma členství ve stejné databázi jako data aplikací z následujících důvodů:

- **Udržovatelnost** aplikace, jejichž data se zapouzdřené v jedné databáze je snazší pochopit, udržovat a nasadit než aplikace, která má dva samostatné databáze.
- **Relační Integrity** tím, že tabulky související s členstvím ve stejné databázi jako aplikace tabulky ho je možné stanovit [omezení cizích klíčů](http://en.wikipedia.org/wiki/Foreign_key) mezi primárních klíčů v Členství související tabulky a tabulek souvisejících aplikací.

Oddělení dat úložiště a aplikace uživatele do samostatné databáze má smysl pouze, pokud máte více aplikací, že každý použít samostatné databáze, ale muset sdílet běžné úložiště uživatele.

### <a name="creating-a-database"></a>Vytvoření databáze

Aplikace, kterou jsme byla vytváření od druhého kurzu není potřeba ještě databáze. Potřebujeme jeden nyní, ale pro úložiště uživatelů. Umožňuje vytvořit a pak přidejte do ní schéma vyžaduje `SqlMembershipProvider` zprostředkovatele (viz krok 2).

> [!NOTE]
> V rámci tohoto kurzu řady použijeme [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) databázi k ukládání tabulek naše aplikací a `SqlMembershipProvider` schématu. Toto rozhodnutí byl proveden dvou důvodů: nejprve z důvodu jeho náklady - free - Express Edition je nejvíce readably přístupné verzi systému SQL Server 2005; za druhé mohou být umístěny databáze SQL Server 2005 Express Edition přímo ve webové aplikaci `App_Data` složky, takže je velmi jednoduché balíčku databáze a webové aplikace společně v jednom souboru ZIP a znovu ji nasaďte bez jakékoli speciální instalační pokyny nebo možnosti konfigurace. Pokud chcete sledovat, používáte verzi systému SQL Server Express Edition, klidně. Kroky jsou téměř stejné. `SqlMembershipProvider` Bude fungovat v žádné verzi systému Microsoft SQL Server 2000 a až schématu.


V Průzkumníku řešení klikněte pravým tlačítkem na `App_Data` složky a přidat novou položku. (Pokud se nezobrazí `App_Data` složku ve vašem projektu, klikněte pravým tlačítkem na projekt v Průzkumníku řešení, vyberte Přidat složku ASP.NET a vyberte `App_Data`.) Vyberte z dialogového okna Přidat novou položku, chcete-li přidat novou databázi SQL s názvem `SecurityTutorials.mdf`. V tomto kurzu přidáme `SqlMembershipProvider` schématu do této databáze; v následujících kurzech vytvoříme další tabulky k zaznamenání dat naše aplikace.


[![Přidat novou databázi SQL s názvem SecurityTutorials.mdf databázi do složky App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Obrázek 1**: Přidání nové databáze SQL název `SecurityTutorials.mdf` do databáze `App_Data` složky ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))


Přidání databáze do `App_Data` složky automaticky zahrne v zobrazení Průzkumník databáze. (Ve verzi Express Edition sady Visual Studio, se nazývá Průzkumník databáze Průzkumníka serveru.) Přejděte do Průzkumníka databáze a rozbalte právě přidané `SecurityTutorials` databáze. Pokud nevidíte Průzkumník databáze na obrazovce, přejděte do nabídky Zobrazit a vybrat Průzkumník databáze nebo stiskněte tlačítko Ctrl + Alt + S. Jak je vidět na obrázku 2, `SecurityTutorials` databáze je prázdná – obsahuje žádné tabulky, žádná zobrazení a žádné uložené procedury.


[![Databázi SecurityTutorials je teď prázdný](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Obrázek 2**: `SecurityTutorials` databáze je aktuálně prázdná ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Krok 2: Přidání`SqlMembershipProvider`schématu do databáze

`SqlMembershipProvider` Vyžaduje konkrétní sadu tabulek, zobrazení a uložených procedur být nainstalovaný v úložišti databáze uživatelů. Tyto objekty požadavků databázi lze přidat pomocí [ `aspnet_regsql.exe` nástroj](https://msdn.microsoft.com/library/ms229862.aspx). Tento soubor je umístěný v `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` složky.

> [!NOTE]
> `aspnet_regsql.exe` Nástroj nabízí funkce příkazový řádek a grafické uživatelské rozhraní. Grafické rozhraní je uživatelsky přívětivější a co vyzkoušíme v tomto kurzu. Rozhraní příkazového řádku je užitečné při přidání `SqlMembershipProvider` musí se schéma je možné automatizovat, například sestavení skriptů nebo automatizované testování scénáře.


`aspnet_regsql.exe` Nástroj se používá k přidání nebo odebrání *aplikačními službami ASP.NET* zadanou databázi serveru SQL Server. Schémata pro zahrnovat služby aplikace ASP.NET `SqlMembershipProvider` a `SqlRoleProvider`, společně s schémata pro zprostředkovatele na základě SQL pro ostatní rozhraní ASP.NET 2.0. Je potřeba zadat dva bity informace, které `aspnet_regsql.exe` nástroje:

- Jestli chcete přidat nebo odebrat aplikační služby, a
- Databáze, ze kterého chcete přidat nebo odebrat schématu služby aplikací

V zobrazení výzvy k databázi chcete použít, `aspnet_regsql.exe` nástroj požádá, abychom mohli poskytovat název serveru databáze nachází na zabezpečovací pověření pro připojení k databázi a název databáze. Pokud používáte jiný - Express edice SQL serveru, musí již víte, tyto informace, jako je stejné informace, které je nutné zadat pomocí připojovacího řetězce při práci s databází přes webovou stránku ASP.NET. Určení názvu serveru a databáze při použití v databázi SQL Server 2005 Express Edition `App_Data` složky, je však složitější.

V následující části prozkoumá snadný způsob, jak zadat název serveru a databáze pro databázi SQL Server 2005 Express Edition v `App_Data` složky. Pokud nepoužíváte, můžete přeskočit na instalace SQL serveru 2005 Express Edition chování části aplikačních služeb.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Určení serveru a název databáze pro databázi SQL Server 2005 Express Edition v`App_Data`složky

Chcete-li použít `aspnet_regsql.exe` nástroj, je potřeba znát název serveru a databáze. Název serveru není `localhost\InstanceName`. Nejpravděpodobnější, *InstanceName* je `SQLExpress`. Ale pokud jste nainstalovali SQL Server 2005 Express Edition ručně (to znamená, že jste nenainstalovali ji automaticky při instalaci sady Visual Studio), je možné, že jste vybrali jiný název instance.

Název databáze je trochu trickier k určení. Databáze v `App_Data` složky mají obvykle název databáze, která zahrnuje [globálně jedinečný identifikátor](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) a cesty k souboru databáze. Je potřeba určit tento název databáze, aby bylo možné přidat schématu aplikací služby prostřednictvím `aspnet_regsql.exe`.

Nejjednodušší způsob, jak zjistit název databáze je prozkoumat pomocí SQL Server Management Studio. SQL Server Management Studio poskytuje grafické rozhraní pro správu databáze SQL Server 2005, ale není dodávají spolu s Express edice systému SQL Server 2005. Dobrá zpráva je, že [si můžete stáhnout](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) volné Express edice systému SQL Server Management Studio.

> [!NOTE]
> Pokud máte také Express Edition verze systému SQL Server 2005 nainstalovaná na pracovní ploše plnou verzi Management Studio pravděpodobně nainstalován. Chcete-li zjistit název databáze, následující stejný postup, jak je uvedeno níže pro edici Express můžete na plnou verzi.


Spusťte ukončením Visual Studio zajistit, aby byly zavřeny všechny zámky vyvolané sady Visual Studio na soubor databáze. Potom spusťte SQL Server Management Studio a připojte se k `localhost\InstanceName` databáze pro SQL Server 2005 Express Edition. Jak již bylo uvedeno dříve, je možné se název instance je `SQLExpress`. Pro možnost ověřování vyberte ověřování systému Windows.


[![Připojte se k instanci SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Obrázek 3**: připojení k instanci serveru SQL Server 2005 Express Edition ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))


Po připojení k instanci SQL Server 2005 Express Edition, zobrazí Management Studio složky pro databáze, nastavení zabezpečení, Server objekty a tak dále. Pokud rozbalíte kartě databáze zobrazí se `SecurityTutorials.mdf` databáze je *není* zaregistrovaný v instanci databáze – potřebujeme nejprve připojit databázi.

Klikněte pravým tlačítkem na složku databází a vyberte připojit v místní nabídce. Tato akce zobrazí dialogové okno připojení databáze. Zde klikněte na tlačítko Přidat, přejděte na `SecurityTutorials.mdf` databáze a klikněte na tlačítko OK. Obrázek 4 ukazuje dialogové okno Připojit databáze po `SecurityTutorials.mdf` databáze byla vybrána. Obrázek 5 ukazuje Průzkumník objektů Management Studio po databáze byla úspěšně připojil.


[![Připojit databázi SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Obrázek 4**: připojení `SecurityTutorials.mdf` databáze ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))


[![Databázi SecurityTutorials.mdf se zobrazí ve složce databáze](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Obrázek 5**: `SecurityTutorials.mdf` databáze se zobrazí ve složce databáze ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))


Jak je vidět na obrázku 5, `SecurityTutorials.mdf` databáze má místo abstruse název. Umožňuje změnit na více snadno zapamatovatelný (a usnadňují zadejte) název. Klikněte pravým tlačítkem na databázi, vyberte v místní nabídce přejmenovat a přejmenujte ji `SecurityTutorialsDatabase`. Nezmění se název souboru, pouze název databáze slouží k identifikaci k systému SQL Server.


[![Přejmenujte databázi SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Obrázek 6**: Přejmenovat databázi do `SecurityTutorialsDatabase`([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))


V tuto chvíli jsme znát název serveru a databáze pro `SecurityTutorials.mdf` souboru databáze: `localhost\InstanceName` a `SecurityTutorialsDatabase`, v uvedeném pořadí. Snažíme se teď připravené k instalaci aplikačních služeb prostřednictvím `aspnet_regsql.exe` nástroj.

### <a name="installing-the-application-services"></a>Instalace aplikačních služeb

Ke spuštění `aspnet_regsql.exe` nástroj, přejděte do nabídky start a vyberte spustit. Zadejte `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` do textového pole a klikněte na tlačítko OK. Alternativně můžete pomocí Průzkumníka Windows přejít k podrobnostem a příslušné složky a dvakrát klikněte na `aspnet_regsql.exe` souboru. Buď přístup budou net stejné výsledky.

Spuštění `aspnet_regsql.exe` nástroj bez argumentů příkazového řádku Spustí grafické uživatelské rozhraní ASP.NET v Průvodci instalací SQL serveru. Průvodce umožňuje snadno přidat nebo odebrat aplikačními službami ASP.NET zadanou databází. Na první obrazovce Průvodce vidět na obrázku 7, popisuje účel tohoto nástroje.


[![Pomocí Průvodce díky ASP.NET SQL Server instalační program přidejte schéma členství](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Obrázek 7**: pomocí ASP.NET SQL Server instalační program Průvodce odešle přidejte schéma členství ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))


Druhý krok v Průvodci zjistím, jestli chcete přidat aplikační služby nebo je odeberte. Vzhledem k tomu, že chceme přidání tabulek, zobrazení a uložených procedur, které jsou nezbytné pro `SqlMembershipProvider`, zvolte možnost konfigurovat SQL Server pro aplikaci služby. Později Pokud chcete odebrat toto schéma z vaší databáze, znovu spusťte tohoto průvodce, ale místo toho zvolíte informace o aplikaci služby odebrat z existující databáze možnost.


[![Zvolte konfiguraci SQL serveru pro možnost aplikace služby](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Obrázek 8**: Zvolte nakonfigurujte systém SQL Server pro aplikaci služby možnost ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))


Třetí krok vyzve k zadání informace o databázi: název serveru, informace o ověřování a název databáze. Pokud jste byli po společně s tohoto kurzu a přidali `SecurityTutorials.mdf` do databáze `App_Data`, připojit se k `localhost\InstanceName`a přejmenoval jej na `SecurityTutorialsDatabase`, potom použijte následující hodnoty:

- Server: `localhost\InstanceName`
- Ověřování systému Windows
- Databáze: `SecurityTutorialsDatabase`


[![Zadejte informace o databázi](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Obrázek 9**: Zadejte informace o databázi ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))


Po zadání informace o databázi, klikněte na tlačítko Další. V posledním kroku shrnuje kroky, které se provedou. K instalaci aplikace služby a pak dokončete průvodce kliknutím na tlačítko Další.

> [!NOTE]
> Pokud jste použili Management Studio a připojte databázi přejmenujte soubor databáze, nezapomeňte odpojení dané databáze a znovu otevřete Visual Studio zavřít Management Studio. K odpojení `SecurityTutorialsDatabase` databáze, klikněte pravým tlačítkem na název databáze a v nabídce úlohy, zvolte Odpojit.


Po dokončení průvodce se vraťte k sadě Visual Studio a přejděte do Průzkumníka databáze. Rozbalte složku tabulky. Měli byste vidět řadu tabulek, jejichž názvy začínají předponou `aspnet_`. Podobně různých zobrazení a uložených procedur najdete v části složky zobrazení a uložených procedur. Tyto objekty databáze tvoří schématu služby aplikací. Vyzkoušíme členství a role konkrétní databázové objekty v kroku 3.


[![Různých tabulek, zobrazení a uložených procedur jsou přidané do databáze](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Obrázek 10**: A různých tabulek, zobrazení a uložené procedury byly přidány do databáze ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe` Grafické uživatelské rozhraní nástroje nainstaluje schéma celá aplikace služby. Ale při provádění `aspnet_regsql.exe` z příkazového řádku můžete zadat, jaký konkrétní aplikace součásti instalace (nebo odebrání) služby. Proto, pokud chcete přidat pouze tabulky, zobrazení a uložené procedury, které jsou nezbytné pro `SqlMembershipProvider` a `SqlRoleProvider` poskytovatelů, spouštění `aspnet_regsql.exe` z příkazového řádku. Alternativně můžete ručně spustit příslušné podmnožinu T-SQL vytvořit skripty, které používá `aspnet_regsql.exe`. Tyto skripty jsou umístěné v `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` složky s názvy jako `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`a tak dále.


V tuto chvíli jsme vytvořili databázové objekty, které vyžaduje `SqlMembershipProvider`. Však ještě musíme pokyn rozhraní členství, že mají používat `SqlMembershipProvider` (versus, můžete, `ActiveDirectoryMembershipProvider`) a že `SqlMembershipProvider` by měl používat `SecurityTutorials` databáze. Podíváme určení zprostředkovatele, používat a přizpůsobení nastavení vybraného zprostředkovatele v kroku 4. Ale nejdřív Podívejme hlubší pohled na databázové objekty, které byly právě vytvořili.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Krok 3: Podívejte se na základní tabulky na schéma

Při práci s rozhraními členství a rolí v aplikaci ASP.NET, jsou podrobnosti implementace zapouzdřené zprostředkovatelem. V budoucích kurzy jsme bude rozhraní s tyto architektury prostřednictvím rozhraní .NET Framework `Membership` a `Roles` třídy. Při použití těchto vysoké úrovně rozhraní API jsme nemusíte týkají označována s podrobnostmi nízké úrovně, jako jsou vykonány jaké dotazy a jaké tabulky jsou změnit pomocí `SqlMembershipProvider` a `SqlRoleProvider`.

Zadána to jsme mohli bez obav použít rozhraní členství a role bez nutnosti prozkoumali schéma databáze vytvořené v kroku 2. Ale při vytváření tabulky k ukládání dat aplikací může musíme vytvořit entity, které se vztahují na uživatele nebo role. Pomáhá tak, aby měl znalost `SqlMembershipProvider` a `SqlRoleProvider` schémata při vytváření cizího klíče mezi tabulkami dat aplikací a tyto tabulky vytvořili v kroku 2 omezení. Kromě toho v některých výjimečných případech může musíme rozhraní s uživatelem a ukládá role přímo na úrovni databáze (místo prostřednictvím `Membership` nebo `Roles` třídy).

### <a name="partitioning-the-user-store-into-applications"></a>Vytváření oddílů úložiště uživatele do aplikací

Rozhraní členství a role jsou navrženy tak, že jedno úložiště uživatele a role mohou být sdílená mezi mnoha různými aplikacemi. Aplikace technologie ASP.NET, který používá rozhraní členství nebo rolí musíte zadat jaké oddílu aplikace použít. Stručně řečeno několika webových aplikací můžete použít stejné úložiště uživatelů a rolí. Uživatele a role úložišť, která jsou rozdělit na tři aplikace znázorňuje obrázek 11: HRSite, CustomerSite a SalesSite. Tyto tři webové aplikace každý mají své vlastní jedinečných uživatelů a rolí, ale všechny fyzicky ukládat své informace o účtu a role uživatele ve stejné databázových tabulek.


[![Uživatelské účty může rozděleného mezi více aplikacemi](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Obrázek 11**: uživatelské účty může být rozdělena na oddíly napříč více aplikacemi ([Kliknutím zobrazit obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))


`aspnet_Applications` Tabulka je co definuje tyto oddíly. Každá aplikace, která používá databázi k ukládání informací o uživatelském účtu je reprezentována řádek v této tabulce. `aspnet_Applications` Tabulka obsahuje čtyři sloupce: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, a `Description`. `ApplicationId` je typu [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) a je primární klíč v tabulce; `ApplicationName` poskytuje jedinečný lidské popisný název pro každou aplikaci.

Členství a Role související tabulky propojit zpět `ApplicationId` pole `aspnet_Applications`. Například `aspnet_Users` tabulky a který obsahuje záznam pro každý uživatelský účet, má `ApplicationId` pole cizího klíče; viz výše pro `aspnet_Roles` tabulky. `ApplicationId` Pole v těchto tabulkách určuje oddílu aplikace uživatelský účet nebo role patří.

### <a name="storing-user-account-information"></a>Ukládání informací o uživatelském účtu

Informace o uživatelském účtu je umístěna ve dvou tabulek: `aspnet_Users` a `aspnet_Membership`. `aspnet_Users` Tabulka obsahuje pole, které obsahují informace nezbytné uživatelského účtu. Jsou tři nejpodstatnější sloupce:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` je primární klíč (a typu `uniqueidentifier`). `UserName` je typu `nvarchar(256)` a společně s heslo, tvoří přihlašovacích údajů uživatele. (Heslo uživatele je uložený v `aspnet_Membership` tabulku.) `ApplicationId` odkazuje na uživatelský účet na konkrétní aplikace v `aspnet_Applications`. Je složené [ `UNIQUE` omezení](https://msdn.microsoft.com/library/ms191166.aspx) na `UserName` a `ApplicationId` sloupce. To zajistí, že v dané aplikaci každé uživatelské jméno je jedinečný, ale umožňuje pro stejné `UserName` pro použití v různých aplikací.

`aspnet_Membership` Tabulka obsahuje další informace o uživatelském účtu, jako je heslo uživatele, e-mailovou adresu, poslední přihlášení datum a čas a tak dále. Je mezi záznamy v souvislosti `aspnet_Users` a `aspnet_Membership` tabulky. Tento vztah je zajištěna `UserId` pole `aspnet_Membership`, který slouží jako primární klíč tabulky. Podobně jako `aspnet_Users` tabulky, `aspnet_Membership` zahrnuje `ApplicationId` pole, která sváže tyto informace do oddílu aplikace.

### <a name="securing-passwords"></a>Zabezpečení hesla

Heslo informace jsou uloženy v `aspnet_Membership` tabulky. `SqlMembershipProvider` Umožňuje hesel se neukládají v databázi pomocí jedné z následujících tří postupů:

- **Zrušte** -je heslo uloženo v databázi jako prostý text. I důrazně bránit použití této možnosti. Pokud databáze dojde k ohrožení bezpečnosti - se hacker, kdo najde zadní vrátka nebo propuštěný zaměstnanec, který má přístup k databázi - existují každých jednoho uživatele přihlašovací údaje pro přístup k.
- **Rozdělí** -jsou hesla hashována pomocí jednocestného algoritmu hash a náhodně generované hodnoty řetězce salt. Tato hodnota hash (spolu s salt) je uložen v databázi.
- **Šifrované** -zašifrovaná verze heslo uloženo v databázi.

Použitá metoda heslo úložiště závisí na `SqlMembershipProvider` nastavení zadané v `Web.config`. Přizpůsobení se podíváme `SqlMembershipProvider` nastavení v kroku 4. Výchozí chování je uložení hodnoty hash hesla.

Sloupce zodpovědná za uložení hesla `Password`, `PasswordFormat`, a `PasswordSalt`. `PasswordFormat` je pole typu `int` techniku, používá se pro ukládání heslo označuje, jehož hodnota: 0 pro zaškrtnutí, 1 pro Hashed; 2 pro šifrovaný. `PasswordSalt` je přiřazen náhodně generované řetězec bez ohledu na to technika úložiště heslo, používat; Hodnota `PasswordSalt` se používá pouze při výpočtu hodnoty hash hesla. Nakonec `Password` sloupec obsahuje data vlastní heslo, je jej heslo v prostém textu, hodnota hash hesla nebo zašifrované heslo.

Tabulka 1 ukazuje, jak tyto tři sloupce může vypadat pro různé postupy úložiště při ukládání hesel MySecret! .

| **Storage Technique&lt;\_o3a\_p /&gt;** | **Heslo&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Zrušte zaškrtnutí | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| S použitím algoritmu hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Šifrované | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabulka 1**: ukázkové hodnoty související s hesly pole při ukládání MySecret heslo!

> [!NOTE]
> Konkrétní šifrování nebo používá algoritmus hash `SqlMembershipProvider` je určen podle nastavení v `<machineKey>` elementu. Jsme probrali tento element konfigurace v kroku 3 <a id="Tutorial3"> </a> [ *konfiguraci ověřování formulářů a rozšířené témata* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) kurzu.


### <a name="storing-roles-and-role-associations"></a>Ukládání role a Role přidružení

Rozhraní framework role vývojářům umožňuje definovat sadu rolí a určit, co uživatelé patří do jaké role. Tyto informace v databázi pomocí dvou tabulek zachycenou: `aspnet_Roles` a `aspnet_UsersInRoles`. Každý záznam v `aspnet_Roles` tabulka představuje roli pro konkrétní aplikace. Podobně jako `aspnet_Users` tabulky, `aspnet_Roles` tabulka má tři sloupce, které jsou relevantní pro naše diskusní:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` je primární klíč (a typu `uniqueidentifier`). `RoleName` je typu `nvarchar(256)`. A `ApplicationId` odkazuje na uživatelský účet na konkrétní aplikace v `aspnet_Applications`. Je složené `UNIQUE` omezení `RoleName` a `ApplicationId` sloupců, zajišťující, že v dané aplikaci je jedinečný název každé role.

`aspnet_UsersInRoles` Tabulka slouží jako mapování mezi uživateli a role. Jsou pouze dva sloupce - `UserId` a `RoleId` - a společně tvoří složené primární klíč.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Krok 4: Určení poskytovatele a přizpůsobení nastavení

Všechny architektury, které podporují modelu poskytovatelů – například členství a rolí architektury - nedostatku podrobnosti implementace sami a místo toho tuto delegovat na třídu zprostředkovatele. V případě rozhraní členství `Membership` třída definuje rozhraní API pro správu uživatelských účtů, ale není komunikovat přímo s úložištěm všechny uživatele. Místo toho `Membership` metody třídy můžete předat požadavek na nakonfigurovaného zprostředkovatele - použijeme `SqlMembershipProvider`. Když jsme jednu z metod v vyvolání `Membership` třídu, jak členství v rámci vědět o delegování volání `SqlMembershipProvider`?

`Membership` Třída má [ `Providers` vlastnost](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) obsahující odkaz na všechny dostupné pro použití třídy registrované zprostředkovatele rámcem členství. Každý registrovaný poskytovatel má přidruženému názvu a typu. Název nabízí lidské friendly způsob, jak odkazovat na konkrétní zprostředkovatele v `Providers` kolekce, když typ identifikuje třída zprostředkovatele. Navíc může každý registrovaný poskytovatel zahrnují nastavení konfigurace. Konfigurace nastavení pro rozhraní členství zahrnují `passwordFormat` a `requiresUniqueEmail`, mezi mnohé další. Tabulka 2 najdete úplný seznam nastavení konfigurace používané `SqlMembershipProvider`.

`Providers` Obsah vlastnosti jsou určeny pomocí nastavení konfigurace webové aplikace. Všechny webové aplikace mají jako výchozí zprostředkovatel s názvem `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`. Tento výchozí zprostředkovatel členství je zaregistrován v `machine.config` (umístěné v `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Jako značka výš ukazuje [ `<membership>` element](https://msdn.microsoft.com/library/1b9hw62f.aspx) definuje nastavení konfigurace pro rozhraní členství při [ `<providers>` podřízený element](https://msdn.microsoft.com/library/6d4936ht.aspx) určuje zaregistrovanou zprostředkovatelé. Zprostředkovatelé lze přidat nebo odebrat pomocí [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) nebo [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementy; použijte [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) elementu, který chcete odebrat všechny aktuálně Registrovaní zprostředkovatelé. Jako značka výš ukazuje `machine.config` Přidá zprostředkovatele s názvem `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`.

Kromě `name` a `type` atributy, `<add>` element obsahuje atributy, které definovat hodnoty pro různá nastavení konfigurace. Tabulka 2 uvádí dostupných `SqlMembershipProvider`-specifické nastavení, spolu s jejich popis.

> [!NOTE]
> Všechny výchozí hodnoty uvedené v tabulce 2 odkazují na výchozí hodnoty definované v `SqlMembershipProvider` třídy. Všimněte si, že ne všechna nastavení konfigurace v `AspNetSqlMembershipProvider` odpovídají na výchozí hodnoty `SqlMembershipProvider` třídy. Pokud nebyly zadány v zprostředkovatele členství, například `requiresUniqueEmail` nastavení výchozí nastavení na hodnotu true. Ale `AspNetSqlMembershipProvider` přepíše tuto výchozí hodnotu podle explicitně zadáte třeba hodnotu `false`.


| **Nastavení&lt;\_o3a\_p /&gt;** | **Popis&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Pro vyvolání umožňující rozhraní členství pro jednoho uživatele úložiště k rozdělení na oddíly napříč různými aplikacemi. Toto nastavení určuje název oddílu aplikace používá zprostředkovatele členství. Pokud tato hodnota není zadaná explicitně, nastavte v době běhu na hodnotu cesta virtuální kořenový adresář aplikace. |
| `commandTimeout` | Určuje hodnota časového limitu příkazů SQL (v sekundách). Výchozí hodnota je 30. |
| `connectionStringName` | Název připojovacího řetězce v `<connectionStrings>` elementu, který chcete použít pro připojení k databázi úložiště uživatele. Tato hodnota je vyžadována. |
| `description` | Obsahuje popis lidské friendly registrované zprostředkovatele. |
| `enablePasswordRetrieval` | Určuje, zda uživatelé mohou načítat jejich zapomenuté heslo. Výchozí hodnota je `false`. |
| `enablePasswordReset` | Určuje, zda uživatelé mohou obnovit své heslo. Použije se výchozí hodnota `true`. |
| `maxInvalidPasswordAttempts` | Maximální počet pokusů o neúspěšných přihlášení, které mohou nastat pro daného uživatele během stanovených `passwordAttemptWindow` před zablokováním uživatele. Výchozí hodnota je 5. |
| `minRequiredNonalphanumericCharacters` | Minimální počet jiných než alfanumerických znaků, které musí být zadány v heslo uživatele. Tato hodnota musí být mezi 0 a 128. Výchozí hodnota je 1. |
| `minRequiredPasswordLength` | Minimální počet znaků požadovaných v heslo. Tato hodnota musí být mezi 0 a 128. Výchozí hodnota je 7. |
| `name` | Název registrované zprostředkovatele. Tato hodnota je vyžadována. |
| `passwordAttemptWindow` | Počet minut, během kterého se nezdařila se sledují pokusů o přihlášení. Pokud uživatel zadává neplatné přihlašovací údaje `maxInvalidPasswordAttempts` zadány časy v rámci toto okno, jsou uzamčen. Výchozí hodnota je 10. |
| `PasswordFormat` | Formát úložiště heslo: `Clear`, `Hashed`, nebo `Encrypted`. Výchozí hodnota je `Hashed`. |
| `passwordStrengthRegularExpression` | Pokud je k dispozici, tento regulární výraz se používá k vyhodnocení sílu vybrané heslo uživatele při vytvoření nového účtu nebo při změně hesla. Výchozí hodnota je prázdný řetězec. |
| `requiresQuestionAndAnswer` | Určuje, zda musí uživatel odpovědět, jeho bezpečnostní otázku při načítání nebo obnovení jeho hesla. Výchozí hodnota je `true`. |
| `requiresUniqueEmail` | Určuje, zda všechny uživatelské účty v dané aplikaci oddílu musí mít jedinečnou e-mailovou adresu. Výchozí hodnota je `true`. |
| `type` | Určuje typ poskytovatele. Tato hodnota je vyžadována. |

**Tabulka 2**: členství a `SqlMembershipProvider` nastavení konfigurace

Kromě `AspNetSqlMembershipProvider`, může být registrován dalších zprostředkovatelů členství na základě aplikací pomocí aplikace přidáním podobně jako značku pro `Web.config` souboru.

> [!NOTE]
> Rozhraní role funguje prakticky stejně: je výchozí poskytovatel registrované role v `machine.config` a registrovaní zprostředkovatelé lze přizpůsobit pro jednotlivé aplikace aplikace v `Web.config`. Vyzkoušíme rozhraní role a jeho konfigurace značek podrobně budoucí kurzu.


### <a name="customizing-thesqlmembershipprovidersettings"></a>Přizpůsobení`SqlMembershipProvider`nastavení

Výchozí hodnota `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) má jeho `connectionStringName` atribut nastaven na `LocalSqlServer`. Podobně jako `AspNetSqlMembershipProvider` poskytovatele, název připojovacího řetězce `LocalSqlServer` je definována v `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Jak vidíte, definuje tento připojovací řetězec SQL 2005 Express Edition se databáze nachází na | DataDirectory|aspnetdb.mdf. Řetězec | DataDirectory | je přeložená za běhu tak, aby odkazoval `~/App_Data/` directory, proto cestu databáze | DataDirectory|aspnetdb.mdf"přeloží na `~/App_Data` / `aspnet.mdf`.

Pokud jsme nezadali žádné informace o poskytovateli členství v naší aplikaci `Web.config` soubor, aplikace použije výchozí zaregistrovaný zprostředkovatel členství, `AspNetSqlMembershipProvider`. Pokud `~/App_Data/aspnet.mdf` databáze neexistuje, bude modulem runtime ASP.NET automaticky vytvořit a přidat schématu služby aplikací. Jsme ale nechcete použít `aspnet.mdf` databáze; místo toho chcete použít `SecurityTutorials.mdf` databáze jsme vytvořili v kroku 2. Tato úprava se dá udělat v jednom ze dvou způsobů:

- <strong>Zadejte hodnotu</strong><strong>`LocalSqlServer`</strong><strong>název připojovacího řetězce v</strong><strong>`Web.config`</strong><strong>.</strong> Přepsáním `LocalSqlServer` hodnota název připojovacího řetězce v `Web.config`, můžeme použít výchozí zaregistrovaný zprostředkovatel členství (`AspNetSqlMembershipProvider`) a mějte ho správně pracovat `SecurityTutorials.mdf` databáze. Tento přístup je v pořádku, pokud obsahu s nastavením konfigurace určeného `AspNetSqlMembershipProvider`. Další informace o této technice najdete v tématu [Scott Guthrie](https://weblogs.asp.net/scottgu/)na příspěvku na blogu [konfigurace služby ASP.NET 2.0 pomocí SQL Server 2000 nebo SQL Server 2005 aplikace](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Přidat nového poskytovatele registrované typu</strong><strong>`SqlMembershipProvider`</strong><strong>a nakonfigurujte jeho</strong><strong>`connectionStringName`</strong><strong>nastavení tak, aby odkazoval</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>databáze.</strong> Tento přístup je užitečný ve scénářích, kde chcete přizpůsobit další vlastnosti konfigurace kromě připojovací řetězec databáze. V projektech pro vlastní I vždy tuto metodu použijte, z důvodu jeho flexibilitu a přehlednosti.

Před přidáním nové registrované zprostředkovatele, který odkazuje `SecurityTutorials.mdf` databáze, nejprve musíme přidejte hodnotu řetězce odpovídající připojení v `<connectionStrings>` v tématu `Web.config`. Následující kód přidá nový připojovací řetězec s názvem `SecurityTutorialsConnectionString` odkazující SQL Server 2005 Express Edition `SecurityTutorials.mdf` databáze `App_Data` složky.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Pokud používáte soubor alternativní databáze, podle potřeby aktualizujte připojovací řetězec. Další informace o tvořící správný připojovací řetězec, najdete v části [ConnectionStrings.com](http://www.connectionstrings.com/).

Dál přidejte následující kód konfigurace členství pro `Web.config` souboru. Tento kód zaregistruje nového poskytovatele s názvem `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Kromě registrace `SecurityTutorialsSqlMembershipProvider` definuje výše uvedený kód poskytovatele, `SecurityTutorialsSqlMembershipProvider` jako výchozí zprostředkovatel (prostřednictvím `defaultProvider` atribut `<membership>` element). Odvolat, aby rozhraní členství může mít víc registrovaných zprostředkovatelů. Vzhledem k tomu `AspNetSqlMembershipProvider` je registrován jako první zprostředkovatele v `machine.config`, slouží jako výchozího zprostředkovatele, pokud jsme znamenat jinak.

V současné době naše aplikace má dva registrovaní zprostředkovatelé: `AspNetSqlMembershipProvider` a `SecurityTutorialsSqlMembershipProvider`. Ale před registrací `SecurityTutorialsSqlMembershipProvider` zprostředkovatele jsme může mít bylo vymazáno všechny dříve registrovaných zprostředkovatelů přidáním [ `<clear />` element](https://msdn.microsoft.com/library/t062y6yc.aspx) bezprostředně před naše `<add>` elementu. To by Vyčistit `AspNetSqlMembershipProvider` ze seznamu registrovaných zprostředkovatelů, znamená to, že `SecurityTutorialsSqlMembershipProvider` by pouze registrované zprostředkovatele členství. Pokud jsme použili tento postup, pak by musíme označit `SecurityTutorialsSqlMembershipProvider` jako výchozího zprostředkovatele, protože je pouze registrované zprostředkovatele členství. Další informace o používání `<clear />`, najdete v části [pomocí `<clear />` při přidávání poskytovatelů](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Všimněte si, že `SecurityTutorialsSqlMembershipProvider`na `connectionStringName` nastavení odkazy právě přidané `SecurityTutorialsConnectionString` název připojovacího řetězce a že jeho `applicationName` bylo nastaveno na hodnotu SecurityTutorials. Kromě toho `requiresUniqueEmail` nastavení byla nastavena na `true`. Všechny ostatní možnosti konfigurace jsou identické s hodnotami v `AspNetSqlMembershipProvider`. Nebojte se provádět žádné změny konfigurace sem, pokud chcete. Tím, že dvě jiných než alfanumerických znaků místo jednoho nebo zvýšením délka hesla na osm znaků místo 7, například může posílit síly hesla.

> [!NOTE]
> Pro vyvolání umožňující rozhraní členství pro jednoho uživatele úložiště k rozdělení na oddíly napříč různými aplikacemi. Zprostředkovatel členství `applicationName` nastavení určuje, jaké aplikace používá zprostředkovatel při práci s úložišti uživatele. Je důležité explicitně nastavit hodnotu `applicationName` nastavení konfigurace, protože pokud `applicationName` není explicitně nastaven, je přiřazena k virtuální kořenová cesta webové aplikace za běhu. To funguje bez problémů tak dlouho, dokud se nezmění. cesta virtuální kořenový adresář aplikace, ale pokud přesunete aplikaci jinou cestu, `applicationName` příliš změní nastavení. V takovém případě bude zprostředkovatel členství začít pracovat s oddíl jinou aplikaci, než byl dříve používán. Uživatelské účty, které jsou vytvořené před přesunutí se bude nacházet v oddílu jinou aplikaci a tito uživatelé nebudou moci přihlásit do lokality. Podrobné informace v této věci naleznete v tématu [vždy nastavená `applicationName` vlastnost při konfiguraci členství technologie ASP.NET 2.0 a ostatní poskytovatelé](https://weblogs.asp.net/scottgu/443634).


## <a name="summary"></a>Souhrn

V tomto okamžiku máme databáze s nakonfigurovanou aplikačních služeb (`SecurityTutorials.mdf`) a nakonfigurovali naše webové aplikace tak, aby používalo rozhraní členství `SecurityTutorialsSqlMembershipProvider` zprostředkovatele jsme právě zaregistrován. Tato registrovaný poskytovatel je typu `SqlMembershipProvider` a jeho `connectionStringName` nastavit odpovídající připojovací řetězec (`SecurityTutorialsConnectionString`) a jeho `applicationName` explicitně nastavena hodnota.

Snažíme se teď připravené k použití rozhraní členství z naší aplikace. V dalším kurzu vyzkoušíme postup vytvoření nových uživatelských účtů. Následující, že se podíváme na ověřování uživatelů, provádí ověření na základě uživatele a ukládá Další informace související s uživatelem v databázi.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vždy nastaven `applicationName` vlastnost při konfiguraci technologie ASP.NET 2.0 členství a dalších zprostředkovatelů](https://weblogs.asp.net/scottgu/443634)
- [Konfigurace technologie ASP.NET 2.0 aplikace služeb na použít SQL Server 2000 nebo SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Stáhnout systému SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Zkoumání ASP.NET 2.0 s členství, role a profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>` Element pro zprostředkovatele pro členství](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>` – Element](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>` Element pro členství](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Pomocí `<clear />` při přidání zprostředkovatelů](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Práce přímo se `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video školení na témata, které jsou obsažené v tomto kurzu

- [Principy členství v ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Konfigurace SQL kvůli spolupráci se schématy členství](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Změna nastavení členství ve výchozím schématu členství](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Alicja Maziarz. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Next](creating-user-accounts-cs.md)
