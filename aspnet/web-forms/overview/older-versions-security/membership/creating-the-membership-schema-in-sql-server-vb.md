---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Vytvoření schématu členství v SQL serveru (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu se spustí prozkoumáním techniky pro přidání nezbytné schématu do databáze, aby bylo možné používat SqlMembershipProvider. Pod jsme wi...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 62a6113c9ddad1722160c7092308939cf7883588
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756080"
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>Vytvoření schématu členství v SQL serveru (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> V tomto kurzu se spustí prozkoumáním techniky pro přidání nezbytné schématu do databáze, aby bylo možné používat SqlMembershipProvider. Pod budeme zkoumat klíče tabulky ve schématu a diskutovat o jejím účelu a důležitosti. V tomto kurzu končí podívat, jak zjistit, které poskytovatel by měl použít členství v rámci aplikace ASP.NET.


## <a name="introduction"></a>Úvod

Předchozí dva kurzy prozkoumat pomocí ověřování pomocí formulářů k identifikaci návštěvníků webu. Rámce ověření formuláře usnadňuje vývojářům pro přihlášení uživatele na webu a nezapomeňte napříč návštěv stránky prostřednictvím lístků pro ověřování. `FormsAuthentication` Třída obsahuje metody pro generování-the-ticket a přidáním souborů cookie pomocí nastavení návštěvníka. `FormsAuthenticationModule` Kontroluje všechny příchozí požadavky a, uživatelům se platný ověřovací lístek, vytvoří a přidruží `GenericPrincipal` a `FormsIdentity` objektu s aktuálním požadavkem. Ověřování pomocí formulářů je pouze mechanismus pro poskytování ověřovací lístek pro návštěvníka při přihlašování a na následné žádosti, analýze tento lístek určit identitu uživatele. Pro webovou aplikaci pro podporu uživatelských účtů ještě musíme implementovat úložiště uživatele a přidat funkci do ověřit přihlašovací údaje, registrovat nové uživatele a velkého počtu další úlohy související s účtem uživatele.

Před ASP.NET 2.0 vývojáři byli hook pro implementaci všechny tyto úlohy související s účtem uživatele. Naštěstí tým ASP.NET rozpoznán tohoto nedostatku a zavedl se členství v rámci s prostředím ASP.NET 2.0. Členství v rámci je sada tříd v rozhraní .NET Framework, které poskytují programový rozhraní pro provádění úlohy související s účtem uživatele core. Toto rozhraní je vytvořeným na základě [modelu poskytovatele](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), které vývojářům umožňuje pružný vlastní implementace standardizované rozhraní API.

Jak je popsáno v <a id="Tutorial1"> </a> [ *Základy zabezpečení a podpora ASP.NET* ](../introduction/security-basics-and-asp-net-support-vb.md) kurzu rozhraní .NET Framework se dodává s dvě předdefinované zprostředkovatele členství: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) a [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Jak již název napovídá, `SqlMembershipProvider` používá databázi serveru Microsoft SQL Server jako úložiště uživatele. Chcete-li použít tento poskytovatel v aplikaci, potřebujeme sdělte poskytovateli jaké databáze pro použití jako úložiště. Jak může Představte si, `SqlMembershipProvider` očekává, že uživatel databáze úložiště má určitá databázových tabulek, zobrazení a uložených procedur. Potřebujeme přidat tohle schéma očekávané k vybrané databázi.

V tomto kurzu začíná tím, že kontroluje techniky pro přidání nezbytné schéma do databáze, chcete-li použít `SqlMembershipProvider`. Pod budeme zkoumat klíče tabulky ve schématu a diskutovat o jejím účelu a důležitosti. V tomto kurzu končí podívat, jak zjistit, které poskytovatel by měl použít členství v rámci aplikace ASP.NET.

Pusťme se do práce!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Krok 1: Rozhodování, kam umístit Store uživatele

Aplikace technologie ASP.NET data jsou obvykle uložena v několik tabulek v databázi. Při implementaci `SqlMembershipProvider` schéma databáze jsme musíte rozhodnout, jestli se má umístit schématu členství ve stejné databázi jako data aplikací nebo do alternativní databáze.

Můžu jenom doporučit vyhledání schématu členství ve stejné databázi jako data aplikací z následujících důvodů:

- **Udržovatelnost** aplikace, jejichž data zapouzdřena v jedné databázi je lépe pochopit, správě a nasazení než aplikace, která má dvě samostatné databáze.
- **Relační Integrity** vyhledáním tabulky související s členstvím ve stejné databázi jako aplikace tabulky ji je možné navázat [omezení cizího klíče](http://en.wikipedia.org/wiki/Foreign_key) mezi primárních klíčů v Členství související tabulky a tabulky souvisejících aplikací.

Oddělení úložiště a aplikace data uživatele do samostatné databáze pouze dává smysl, pokud máte více aplikací, že každé použití samostatných databází, ale muset sdílejí společné úložiště uživatele.

### <a name="creating-a-database"></a>Vytvoření databáze

Aplikace, kterou jsme se vytváření od druhé části kurzu není potřeba ještě databáze. Potřebujeme, ale pro úložiště uživatelů. Pojďme vytvořit a pak přidejte do ní schéma vyžadované `SqlMembershipProvider` zprostředkovatele (viz krok 2).

> [!NOTE]
> V celé této sérii kurzů použijeme [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) databázi pro ukládání tabulky našich aplikací a `SqlMembershipProvider` schématu. Toto rozhodnutí bylo dvou důvodů: nejprve z důvodu jeho cena – zdarma – edice Express je nejvíce readably přístupnou verzi systému SQL Server 2005; za druhé, databáze SQL Server 2005 Express Edition je možné použít přímo ve webové aplikaci `App_Data` složky, takže díky balení databáze a webové aplikace společně v jednom souboru ZIP a znovu ji nasadíte bez jakékoli speciální instalační pokyny nebo možnosti konfigurace. Pokud chcete postupovat s námi používáte verzi systému SQL Server Express Edition, můžete. Postup je prakticky totožný. `SqlMembershipProvider` Bude fungovat v žádné verzi systému Microsoft SQL Server 2000 a až schématu.

V Průzkumníku řešení klikněte pravým tlačítkem na `App_Data` složce a chcete přidat novou položku. (Pokud se nezobrazí `App_Data` složku ve vašem projektu, klikněte pravým tlačítkem na projekt v Průzkumníku řešení vyberte Přidat složku ASP.NET a vybrat `App_Data`.) Z dialogového okna Přidat novou položku zvolte Přidat novou databázi SQL s názvem `SecurityTutorials.mdf`. V tomto kurzu přidáme `SqlMembershipProvider` schématu pro tuto databázi, v následujících kurzech vytvoříme další tabulky k zaznamenání dat o našich aplikací.


[![Přidat novou databázi SQL s názvem SecurityTutorials.mdf databáze do složky App_Data](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Obrázek 1**: přidejte nový název databáze SQL `SecurityTutorials.mdf` databáze `App_Data` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


Přidání databáze do `App_Data` složku automaticky zahrne v zobrazení Průzkumník databáze. (Ve verzi Express Edition sady Visual Studio, se nazývá Průzkumník databáze v Průzkumníku serveru.) Přejít na Průzkumník databáze a rozbalte právě přidané `SecurityTutorials` databáze. Pokud se nezobrazí Průzkumník databáze na obrazovce, přejděte do zobrazení nabídky a zvolte Průzkumník databáze nebo stiskněte kombinaci kláves Ctrl + Alt + S. Obrázek 2 ukazuje, `SecurityTutorials` databáze je prázdná – neobsahuje žádné tabulky, k dispozici žádná zobrazení a žádné uložené procedury.


[![SecurityTutorials databáze je aktuálně prázdný](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Obrázek 2**: `SecurityTutorials` databáze je aktuálně prázdný ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Krok 2: Přidání`SqlMembershipProvider`schéma do databáze

`SqlMembershipProvider` Vyžaduje konkrétní sadu tabulek, zobrazení a uložených procedur nainstalovaný v uživatelské databázi úložiště. Tyto objekty požadované databáze můžete přidat pomocí [ `aspnet_regsql.exe` nástroj](https://msdn.microsoft.com/library/ms229862.aspx). Tento soubor je umístěn v `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` složky.

> [!NOTE]
> `aspnet_regsql.exe` Nabízí nástroj pro příkazový řádek a grafické uživatelské rozhraní. Grafické rozhraní je uživatelsky přívětivější a co prozkoumáme v tomto kurzu. Rozhraní příkazového řádku je užitečné, když přidání `SqlMembershipProvider` schématu musí být automatické, jako je například sestavení skripty nebo automatizované testování scénářů.

`aspnet_regsql.exe` Nástroj se používá k přidání nebo odebrání *aplikačními službami ASP.NET* k zadané databázi SQL serveru. Zahrnovat schémata pro aplikační služby technologie ASP.NET `SqlMembershipProvider` a `SqlRoleProvider`, spolu s schémata pro zprostředkovatele založený na SQL pro jiná rozhraní ASP.NET 2.0. Potřebujeme pro poskytování informací, které mají dva bity `aspnet_regsql.exe` nástroje:

- Určuje, zda chcete přidat nebo odebrat aplikace služby a
- Databáze, ze kterého chcete přidat nebo odebrat aplikace služby schématu

V vás vyzve k zadání databáze, kterou chcete použít, `aspnet_regsql.exe` nástroj dotazem, abychom mohli poskytovat název serveru databáze nachází na zabezpečovací přihlašovací údaje pro připojení k databázi a název databáze. Pokud použijete jiné - Express edici systému SQL Server, musí už znáte tyto informace, jako je stejná informace, je třeba zadat pomocí připojovacího řetězce při práci s databází přes webové stránky ASP.NET. Určení názvu serveru a databáze při použití v databázi SQL Server 2005 Express Edition `App_Data` složka, ale je trochu složitější.

Následující oddíl se zabývá jednoduchý způsob, jak zadat název serveru a databáze pro databázi SQL Server 2005 Express Edition v `App_Data` složky. Pokud nechcete použít SQL Server 2005 Express Edition klidně můžete přeskočit přímo k instalace v části aplikační služby.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Určení serveru a název databáze pro databázi SQL Server 2005 Express Edition v`App_Data`složky

Chcete-li použít `aspnet_regsql.exe` nástroj budeme potřebovat znát název serveru a databáze. Název serveru je `localhost\InstanceName`. Pravděpodobně, *InstanceName* je `SQLExpress`. Nicméně pokud jste nainstalovali ručně SQL Server 2005 Express Edition (to znamená, že jste nenainstalovali ho automaticky při instalaci sady Visual Studio), je možné, že jste vybrali jiný název instance.

Název databáze je o něco trickier určit. Databáze ve `App_Data` složky mají obvykle název databáze, která zahrnuje [globálně jedinečný identifikátor](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) spolu se cesta k souboru databáze. Je třeba určit název této databáze, chcete-li přidat schématu služby aplikací prostřednictvím `aspnet_regsql.exe`.

Nejjednodušší způsob, jak zjistit název databáze je prozkoumat SQL Server Management Studio. SQL Server Management Studio poskytuje grafické rozhraní pro správu databáze systému SQL Server 2005, ale ne dodávají spolu s Express edice systému SQL Server 2005. Dobrou zprávou je, že [si můžete stáhnout](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) na bezplatné Express edice systému SQL Server Management Studio.

> [!NOTE]
> Pokud máte také verze nainstalované na pracovní ploše, plnou verzi aplikace Management Studio je pravděpodobně nainstalován systém SQL Server 2005 Express Edition. Chcete-li zjistit název databáze po stejný postup, jak je uvedeno níže pro edice Express, můžete použít na plnou verzi.

Začněte tím, že zavření sady Visual Studio k zajištění, že žádné zámky uložené v souboru databáze aplikace Visual Studio zavřená. V dalším kroku spusťte SQL Server Management Studio a připojte se k `localhost\InstanceName` databáze pro SQL Server 2005 Express Edition. Jak je uvedeno výše, je pravděpodobné, je název instance `SQLExpress`. Možnost ověřování vyberte možnost ověřování Windows.


[![Připojte se k instanci serveru SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Obrázek 3**: Připojte se k instanci serveru SQL Server 2005 Express Edition ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


Po připojení k instanci SQL serveru 2005 Express Edition, Management Studio zobrazí složek pro databáze, nastavení zabezpečení, objekty serveru a tak dále. Pokud rozbalíte na kartě databáze bude uvidíte, že `SecurityTutorials.mdf` databáze je *není* zaregistrovaný v instanci databáze – potřebujeme nejprve připojte databázi.

Klikněte pravým tlačítkem na složku databází a v místní nabídce zvolte možnost připojit. Zobrazí se dialogové okno Připojit databáze. Zde, klikněte na tlačítko Přidat, přejděte `SecurityTutorials.mdf` databáze a klikněte na tlačítko OK. Obrázek 4 ukazuje dialogové okno Připojit databáze po `SecurityTutorials.mdf` byla vybrána databáze. Obrázek 5 ukazuje Průzkumník objektů systému Management Studio po databáze byl úspěšně připojen.


[![Připojte databázi SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Obrázek 4**: připojit `SecurityTutorials.mdf` databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![Databáze SecurityTutorials.mdf zobrazí ve složce databáze](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Obrázek 5**: `SecurityTutorials.mdf` databáze se zobrazí ve složce databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


Jak je vidět na obrázku 5, `SecurityTutorials.mdf` databáze má raději abstruse název. Pojďme jej změnit na víc zapamatovatelnou (a usnadňuje zadejte) název. Klikněte pravým tlačítkem na databázi, zvolte Přejmenovat v místní nabídce a přejmenujte ji `SecurityTutorialsDatabase`. Nezmění se název souboru, pouze název databáze slouží k identifikaci k systému SQL Server.


[![Přejmenování databáze SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Obrázek 6**: databáze, kterou chcete přejmenovat `SecurityTutorialsDatabase`([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


V tuto chvíli jsme znát název serveru a databáze pro `SecurityTutorials.mdf` databázového souboru: `localhost\InstanceName` a `SecurityTutorialsDatabase`v uvedeném pořadí. Máme teď připravena k instalaci aplikace služeb prostřednictvím `aspnet_regsql.exe` nástroj.

### <a name="installing-the-application-services"></a>Instalace aplikační služby

Ke spuštění `aspnet_regsql.exe` nástroj, přejděte do nabídky start a klikněte na tlačítko spustit. Zadejte `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` do textového pole a klikněte na tlačítko OK. Alternativně můžete použít Windows Explorer k podrobnostem a příslušné složky a dvojím kliknutím `aspnet_regsql.exe` souboru. Kterýkoliv přístup bude net stejné výsledky.

Spuštění `aspnet_regsql.exe` grafickém uživatelském rozhraní Průvodce instalací SQL serveru ASP.NET se spustí nástroj bez argumentů příkazového řádku. Průvodce umožňuje snadno přidat nebo odebrat aplikačních služeb technologie ASP.NET v zadané databázi. První obrazovce průvodce, je znázorněno na obrázku 7, jsou popsány nástroje.


[![Slouží k přidání schématu členství využívá Průvodce instalace serveru SQL technologie ASP.NET](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Obrázek 7**: použijte ASP.NET SQL Server nastavení Průvodce provede přidání schématu členství ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


Druhý krok v Průvodci nám zeptá, zda chceme přidat aplikační služby nebo je odeberte. Protože chceme přidat tabulek, zobrazení a uložených procedur, které jsou nezbytné pro `SqlMembershipProvider`, zvolte Konfigurovat systém SQL Server pro aplikace možnost služby. Pokud chcete odebrat toto schéma z databáze, později, spusťte znovu tohoto průvodce, ale místo toho zvolit informace o službách aplikací odebrat z existující možnost databáze.


[![Zvolte konfiguraci serveru SQL pro možnost aplikace služby](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Obrázek 8**: Zvolte konfigurovat systém SQL Server pro aplikaci služby možnost ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


Třetí krok zobrazí výzvu k zadání informace o databázi: název serveru, informace o ověřování a název databáze. Pokud jste postupovali podle spolu se v tomto kurzu a přidali `SecurityTutorials.mdf` databáze `App_Data`, připojit ho k `localhost\InstanceName`a přejmenoval jej na `SecurityTutorialsDatabase`, pak použijte následující hodnoty:

- Server: `localhost\InstanceName`
- Ověřování systému Windows
- Databáze: `SecurityTutorialsDatabase`


[![Zadejte informace o databázi](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Obrázek 9**: Zadejte informace o databázi ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


Jakmile zadáte informace o databázi, klikněte na tlačítko Další. V posledním kroku jsou shrnuté kroky, které se mají provést. Instalace aplikační služby a pak dokončete průvodce, klikněte na tlačítko Další.

> [!NOTE]
> Pokud jste použili Management Studio a připojte databázi přejmenujte soubor databáze, je potřeba odpojit databázi a před otevřením sady Visual Studio zavřít Management Studio. Chcete-li odpojit `SecurityTutorialsDatabase` databáze, klikněte pravým tlačítkem na název databáze a v nabídce úlohy zvolte Odpojit.

Po dokončení Průvodce vraťte se do sady Visual Studio a přejděte do Průzkumníka databáze. Rozbalte složku tabulky. Měli byste vidět řadu tabulek, jejichž názvy začínají předponou `aspnet_`. Obdobně širokou škálu zobrazení a uložených procedur najdete ve složkách zobrazení a uložených procedur. Tyto databázové objekty tvoří schéma služby aplikace. Prozkoumáme databázových objektů konkrétní členství a role v kroku 3.


[![Celou řadu tabulek, zobrazení a uložených procedur jsou přidané do databáze](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Obrázek 10**: A různých tabulek, zobrazení a uložených procedur byly přidány do databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe` Grafické uživatelské rozhraní nástroje instaluje schéma služby celé aplikace. Ale při provádění `aspnet_regsql.exe` z příkazového řádku můžete určit, jaké konkrétní aplikačních služeb součásti k instalaci (nebo odebrání). Proto pokud chcete přidat pouze tabulky, zobrazení a uložené procedury, které jsou nezbytné pro `SqlMembershipProvider` a `SqlRoleProvider` poskytovatelů, spouštění `aspnet_regsql.exe` z příkazového řádku. Alternativně můžete spustit ručně příslušnou podmnožinu jazyka T-SQL vytvořit skripty používané `aspnet_regsql.exe`. Tyto skripty jsou umístěny v `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` složky s názvy jako `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`, a tak dále.

V tuto chvíli jsme vytvořili databázové objekty vyžadované `SqlMembershipProvider`. Však stále potřebujeme dáte pokyn, aby rozhraní členství, by měl použít `SqlMembershipProvider` (oproti, Řekněme, že, `ActiveDirectoryMembershipProvider`) a že `SqlMembershipProvider` používejte `SecurityTutorials` databáze. Podíváme se na určení zprostředkovatele, používat a jak upravit vybraného poskytovatele nastavení v kroku 4. Ale nejprve se podívejme se podrobněji na databázové objekty, které se právě vytvořili.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Krok 3: Podívejte se na základní tabulky schématu

Při práci s rozhraními členství a rolí v aplikaci ASP.NET, podrobnosti implementace jsou zapouzdřeny zprostředkovatelem. V budoucích kurzech jsme se s těmito rozhraními prostřednictvím rozhraní .NET Framework rozhraní `Membership` a `Roles` třídy. Při použití těchto rozhraní API vysoké úrovně jsme nemusíte sami se týkají se podrobnosti nižší úrovně, například jaké dotazy se spouštějí nebo které tabulky se změnil `SqlMembershipProvider` a `SqlRoleProvider`.

To směru, bychom mohli bez obav použít rozhraní členství a rolí bez nutnosti prozkoumali schéma databáze vytvořené v kroku 2. Ale při vytváření tabulky k ukládání aplikačních dat. možná potřebujeme vytvořit entity, které se vztahují na uživatele nebo role. Umožňuje mít znalost `SqlMembershipProvider` a `SqlRoleProvider` schémata při navazování cizího klíče omezení mezi tabulkami dat aplikace a tyto tabulky vytvořené v kroku 2. Kromě toho v některých výjimečných případech může potřebujeme k propojení s uživateli a role uloží přímo na úrovni databáze (namísto prostřednictvím `Membership` nebo `Roles` třídy).

### <a name="partitioning-the-user-store-into-applications"></a>Dělení Store uživatele do aplikací

Členství a rolí rozhraní jsou navržené tak, že jedno úložiště uživatele a roli je možné sdílet mezi mnoha různých aplikací. Aplikace ASP.NET, která používá rozhraní členství nebo rolí, musíte zadat oddílu aplikace používat. Stručně řečeno více webových aplikací můžete použít stejné úložiště pro uživatele a role. Úložiště pro uživatele a role, které jsou rozdělené do tří aplikací znázorňuje obrázek 11: HRSite CustomerSite a SalesSite. Tyto tři webové aplikace každý mají své vlastní jedinečných uživatelů a rolí, ale jsou v nich všechny fyzicky uložené informace o účtu a role uživateli ve stejných databázových tabulkách.


[![Může být dělené uživatelské účty napříč více aplikacemi](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Obrázek 11**: uživatelské účty může být rozdělit na oddíly napříč více aplikacemi ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


`aspnet_Applications` Tabulka je co definuje tyto oddíly. Každá aplikace, která používá databázi k ukládání informací o uživatelském účtu představuje řádek v této tabulce. `aspnet_Applications` Tabulka obsahuje čtyři sloupce: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, a `Description`.`ApplicationId` je typu [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) a primárního klíče v tabulce. `ApplicationName` poskytuje jedinečné lidských – popisný název pro každou aplikaci.

Členství a Role související tabulky odkazují zpátky na `ApplicationId` pole `aspnet_Applications`. Například `aspnet_Users` tabulku, která obsahuje záznam pro každý uživatelský účet má `ApplicationId` pole cizího klíče; viz výše pro `aspnet_Roles` tabulky. `ApplicationId` Pole v těchto tabulkách určuje oddíl aplikace uživatelský účet nebo roli patří.

### <a name="storing-user-account-information"></a>Ukládání informací o uživatelském účtu

Informace o uživatelském účtu je umístěnými ve dvou tabulkách: `aspnet_Users` a `aspnet_Membership`. `aspnet_Users` Tabulka obsahuje pole, které obsahují informace podstatné uživatelského účtu. Jsou tři nejvíce relevantní sloupce:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` představuje primární klíč (a typu `uniqueidentifier`). `UserName` je typu `nvarchar(256)` a společně s heslem, tvoří přihlašovacích údajů uživatele. (Heslo uživatele je uložená v `aspnet_Membership` tabulky.) `ApplicationId` propojí účet uživatele ke konkrétní aplikaci v `aspnet_Applications`. Je složeného [ `UNIQUE` omezení](https://msdn.microsoft.com/library/ms191166.aspx) na `UserName` a `ApplicationId` sloupce. Tím se zajistí, že v dané aplikaci každé uživatelské jméno je jedinečné, ale umožňuje stejné `UserName` pro použití v různých aplikacích.

`aspnet_Membership` Tabulka obsahuje další informace o uživatelském účtu, jako je heslo uživatele, e-mailovou adresu, poslední přihlášení datum a čas a tak dále. Neexistuje shoda mezi záznamy `aspnet_Users` a `aspnet_Membership` tabulky. Tento vztah je zajištěna `UserId` pole `aspnet_Membership`, který slouží jako primární klíč v tabulce. Podobně jako `aspnet_Users` tabulky, `aspnet_Membership` zahrnuje `ApplicationId` pole, která spojují tyto informace do konkrétního oddílu aplikace.

### <a name="securing-passwords"></a>Zabezpečení hesel

Informace o hesle je uložen v `aspnet_Membership` tabulky. `SqlMembershipProvider` Umožňuje hesla uložená v databázi pomocí jedné z následujících tří postupů:

- **Vymazat** – heslo je uloženo v databázi jako prostý text. Můžu důrazně bránit použití této možnosti. Pokud databáze dojde k ohrožení - být hacker nálezci usnadní zadní vrátka nebo propuštěný zaměstnance, který má přístup k databázi – každý jednoho uživatele přihlašovací údaje jsou existuje odběr.
- **Hodnoty hash** – jsou hesla hashována pomocí jednocestného hashovacího algoritmu a náhodně generované hodnoty řetězce salt. Tato hodnota hash (spolu s hodnota salt) je uložen v databázi.
- **Šifrované** -zašifrovaná verze hesla uložená v databázi.

Použitá metoda heslo úložiště závisí `SqlMembershipProvider` nastavení uvedená v `Web.config`. Přizpůsobení se podíváme `SqlMembershipProvider` nastavení v kroku 4. Výchozí chování je ukládat hodnoty hash hesla.

Sloupce za níž byla hesla uložená `Password`, `PasswordFormat`, a `PasswordSalt`. `PasswordFormat` je pole s typem `int` jehož hodnota označuje postup slouží k uložení hesla: 0 pro Clear; 1 pro Hashed; 2 pro šifrované. `PasswordSalt` je přiřazen náhodně generované řetězec bez ohledu na to technika úložiště heslo, používají; Hodnota `PasswordSalt` se používá pouze při výpočtu hodnoty hash hesla. Nakonec `Password` sloupec obsahuje data skutečné heslo, ať už jde heslo jako prostý text, hodnotu hash hesla nebo šifrované heslo.

Tabulka 1 ukazuje, jak tyto tři sloupce může vypadat pro různých technik vytváření úložiště při ukládání hesel MySecret! .

| **Úložiště techniku&lt;\_o3a\_p /&gt;** | **Heslo&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Vymazat | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Hodnoty hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Šifrované | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabulka 1**: ukázkové hodnoty pro pole související s hesly při ukládání MySecret heslo!

> [!NOTE]
> Konkrétní šifrování nebo hashovacího algoritmu používaného `SqlMembershipProvider` je určena nastavením v `<machineKey>` elementu. Jsme probírali tento prvek konfigurace v kroku 3 <a id="Tutorial3"> </a> [ *konfigurace ověřování formulářů a témata pokročilé* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) kurzu.

### <a name="storing-roles-and-role-associations"></a>Ukládání role a přiřazení Role

Role rozhraní framework umožňuje vývojářům definovat sadu rolí a určit, co uživatelé patří do rolích. Tyto informace je zachycena v databázi pomocí dvou tabulkách: `aspnet_Roles` a `aspnet_UsersInRoles`. Každý záznam v `aspnet_Roles` tabulka představuje roli pro konkrétní aplikaci. Podobně jako `aspnet_Users` tabulky, `aspnet_Roles` tabulka obsahuje tři sloupce, které jsou relevantní pro naše diskuse:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` představuje primární klíč (a typu `uniqueidentifier`). `RoleName` je typu `nvarchar(256)`. A `ApplicationId` propojí účet uživatele ke konkrétní aplikaci v `aspnet_Applications`. Je složeného `UNIQUE` omezení `RoleName` a `ApplicationId` sloupců, zajištění, že v dané aplikace je jedinečný název každé role.

`aspnet_UsersInRoles` Tabulka slouží jako mapování mezi uživateli a role. Existují jenom dva sloupce - `UserId` a `RoleId` – a společně tvoří složený primární klíč.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Krok 4: Určení poskytovatele a přizpůsobení nastavení

Všechna rozhraní, které podporují podle modelu poskytovatele – například členství a rolí rozhraní – podrobnosti implementace samy nemají a místo toho delegování zodpovědnosti na třídu poskytovatele. V případě rozhraní členství `Membership` třída definuje rozhraní API pro správu uživatelských účtů, ale nekomunikuje přímo s všechny uživatele v úložišti. Místo toho `Membership` metody třídy můžete předat požadavek na nakonfigurované poskytovateli - použijeme `SqlMembershipProvider`. Když jsme jedné z metod v vyvolat `Membership` třídy, jak členství v rámci vědět o delegování volání `SqlMembershipProvider`?

`Membership` Třída nemá [ `Providers` vlastnost](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) , který obsahuje odkaz na všechny třídy registrovaného zprostředkovatele k dispozici pro použití v rámci rozhraní členství. Každý registrovaný poskytovatel má odpovídající název a typ. Název nabízí lidských přívětivá způsob, jak odkazovat na konkrétní zprostředkovatele v `Providers` kolekce, zatímco identifikuje typ třída zprostředkovatele. Navíc může každý registrovaný poskytovatel zahrnují nastavení konfigurace. Zahrnují nastavení konfigurace pro členství v rámci `PasswordFormat` a `requiresUniqueEmail`, mezi mnoha dalších. Viz tabulka 2 pro úplný seznam nastavení konfigurace používané `SqlMembershipProvider`.

`Providers` Obsah vlastnosti jsou určeny pomocí nastavení konfigurace webové aplikace. Ve výchozím nastavení, mají všechny webové aplikace s názvem zprostředkovatele `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`. Tento výchozí zprostředkovatel členství je registrován v `machine.config` (umístěný ve `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Jako značka výše ukazuje [ `<membership>` element](https://msdn.microsoft.com/library/1b9hw62f.aspx) definuje nastavení konfigurace pro členství v rámci při [ `<providers>` podřízený element](https://msdn.microsoft.com/library/6d4936ht.aspx) určuje zaregistrovanou poskytovatelé. Poskytovatelé mohou být přidány nebo odebrány [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) nebo [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementy; použijte [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) odebrat všechny aktuálně – element registrovaných zprostředkovatelů. Jako značka výše znázorňuje `machine.config` přidá poskytovatele s názvem `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`.

Kromě `name` a `type` atributy, `<add>` element obsahuje atributy, které určují hodnoty pro různá nastavení konfigurace. Tabulka 2 uvádí dostupné `SqlMembershipProvider`-konkrétních nastavení konfigurace, spolu s jejich popis.

> [!NOTE]
> Všechny výchozí hodnoty, které jste si poznamenali v tabulce 2 odkazují na výchozí hodnoty podle `SqlMembershipProvider` třídy. Všimněte si, že ne všechna nastavení konfigurace v `AspNetSqlMembershipProvider` odpovídají výchozí hodnoty `SqlMembershipProvider` třídy. Například, pokud není zadán ve zprostředkovateli členství `requiresUniqueEmail` výchozí nastavení na hodnotu true. Ale `AspNetSqlMembershipProvider` přepisuje tuto výchozí hodnotu tak, že explicitně zadáte hodnotu `false`.

| **Nastavení&lt;\_o3a\_p /&gt;** | **Popis&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Připomínáme, že členství v rámci umožňuje pro jednoho uživatele úložiště k rozdělení na oddíly napříč více aplikacemi. Toto nastavení označuje název oddílu aplikace používá zprostředkovatele členství. Pokud tato hodnota není zadaná explicitně, nastavte v době běhu k hodnotě virtuální kořenová cesta aplikace. |
| `commandTimeout` | Určuje hodnotu časového limitu příkazu SQL (v sekundách). Výchozí hodnota je 30. |
| `connectionStringName` | Název připojovacího řetězce v `<connectionStrings>` prvek, který chcete použít pro připojení k uživatelské databázi úložiště. Tato hodnota je povinná. |
| `description` | Obsahuje popis lidských vhodných registrovaných zprostředkovatelů. |
| `enablePasswordRetrieval` | Určuje, zda uživatelé mohou načítat zapomenuté heslo. Výchozí hodnota je `false`. |
| `enablePasswordReset` | Určuje, jestli uživatelé můžou obnovit své heslo. Výchozí hodnota je `true`. |
| `maxInvalidPasswordAttempts` | Maximální počet neúspěšných pokusů o přihlášení, které mohou nastat pro daného uživatele během stanovených `passwordAttemptWindow` před zablokováním uživatele. Výchozí hodnota je 5. |
| `minRequiredNonalphanumericCharacters` | Minimální počet jiných než alfanumerických znaků, které musí být uvedena v hesla uživatele. Tato hodnota musí být mezi 0 a 128. Výchozí hodnota je 1. |
| `minRequiredPasswordLength` | Minimální počet znaků v heslu. Tato hodnota musí být mezi 0 a 128. Výchozí hodnota je 7. |
| `name` | Název registrovaného zprostředkovatele. Tato hodnota je povinná. |
| `passwordAttemptWindow` | Počet minut, během které se nepodařilo přihlašovací jméno, které jsou sledovány pokusy. Pokud uživatel zadává neplatným přihlašovacím údajům `maxInvalidPasswordAttempts` čas v rámci této zadaný okna, jsou uzamčen. Výchozí hodnota je 10. |
| `PasswordFormat` | Formát ukládání hesel: `Clear`, `Hashed`, nebo `Encrypted`. Výchozí hodnota je `Hashed`. |
| `passwordStrengthRegularExpression` | Pokud je zadán, tento regulární výraz se používá k vyhodnocení sílu vybrané heslo uživatele při vytváření nového účtu nebo při změně hesla. Výchozí hodnota je prázdný řetězec. |
| `requiresQuestionAndAnswer` | Určuje, jestli musí uživatel odpovědět, jeho bezpečnostní otázku při načítání nebo kdo resetuje jeho heslo. Výchozí hodnota je `true`. |
| `requiresUniqueEmail` | Určuje, zda všechny uživatelské účty v oddílu dané aplikace musí mít jedinečnou e-mailovou adresu. Výchozí hodnota je `true`. |
| `type` | Určuje typ zprostředkovatele. Tato hodnota je povinná. |

**Tabulka 2**: členství a `SqlMembershipProvider` nastavení konfigurace

Kromě `AspNetSqlMembershipProvider`, dalších zprostředkovatelů členství se může zaregistrovat na základě aplikace aplikace tak, že přidáte podobné značky `Web.config` souboru.

> [!NOTE]
> Rozhraní role funguje prakticky stejně jako: je výchozí zprostředkovatel registrované rolí v `machine.config` a lze jej přizpůsobit registrovaných zprostředkovatelů pro jednotlivé aplikace pomocí aplikace v `Web.config`. Prozkoumáme role framework a jeho konfigurace značky podrobně v budoucích kurzech.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Přizpůsobení`SqlMembershipProvider`nastavení

Výchozí hodnota `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) má jeho `connectionStringName` atribut nastaven na `LocalSqlServer`. Podobně jako `AspNetSqlMembershipProvider` zprostředkovatele, název připojovacího řetězce `LocalSqlServer` je definována v `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Jak je vidět, definuje tento připojovací řetězec SQL 2005 Express Edition databáze umístěna na | DataDirectory|aspnetdb.mdf. Řetězec | DataDirectory | přeložit za běhu, přejděte `~/App_Data/` adresáře, proto cesta k databázi | DataDirectory|aspnetdb.mdf se přeloží na `~/App_Data` / `aspnet.mdf`.

Pokud jsme nezadali žádné informace o poskytovateli členství v naší aplikaci `Web.config` soubor, aplikace používá výchozí zaregistrovaný zprostředkovatel členství, `AspNetSqlMembershipProvider`. Pokud `~/App_Data/aspnet.mdf` databáze buď neexistuje, automaticky ji vytvoříte a přidáte schématu aplikací služby modulu runtime ASP.NET. Nechceme však použít `aspnet.mdf` databáze; místo toho chceme použít `SecurityTutorials.mdf` databázi jsme vytvořili v kroku 2. Tato změna lze provést jedním ze dvou způsobů:

- <strong>Zadejte hodnotu</strong><strong>`LocalSqlServer`</strong><strong>název připojovacího řetězce v</strong><strong>`Web.config`</strong><strong>.</strong> Přepsáním `LocalSqlServer` hodnotu název připojovacího řetězce v `Web.config`, můžeme použít výchozí zaregistrovaný zprostředkovatel členství (`AspNetSqlMembershipProvider`) a jeho správně pracovat `SecurityTutorials.mdf` databáze. Tento přístup je v pořádku, pokud jste s nastavením konfigurace určené obsahu `AspNetSqlMembershipProvider`. Další informace o této techniky najdete v tématu [Scott Guthrie](https://weblogs.asp.net/scottgu/)pro blogový příspěvek [konfigurace technologie ASP.NET 2.0 aplikačními službami, abyste pomocí SQL Server 2000 nebo SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Přidat nového poskytovatele registrovaného typu</strong><strong>`SqlMembershipProvider`</strong><strong>a nakonfigurovat jeho</strong><strong>`connectionStringName`</strong><strong>nastavení tak, aby odkazoval</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>databáze.</strong> Tento přístup je užitečné v situacích, ve které chcete přizpůsobit další vlastnosti konfigurace kromě připojovací řetězec databáze. V projektech pro vlastní používám vždy tento přístup z důvodu jeho flexibilitu a lepší čitelnost.

Před přidáním nového registrovaného poskytovatele, který odkazuje `SecurityTutorials.mdf` databáze, musíme nejprve přidejte hodnotu řetězce odpovídající připojení v `<connectionStrings>` tématu `Web.config`. Následující kód přidá nový připojovací řetězec s názvem `SecurityTutorialsConnectionString` , která odkazuje na SQL Server 2005 Express Edition `SecurityTutorials.mdf` databáze v `App_Data` složky.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Pokud používáte soubor alternativní databáze, podle potřeby aktualizujte připojovací řetězec. Další informace o tvořící správný připojovací řetězec, najdete [ConnectionStrings.com](http://www.connectionstrings.com/).

V dalším kroku přidejte následující kód konfigurace členství pro `Web.config` souboru. Tento kód zaregistruje nový zprostředkovatel s názvem `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Kromě registrace `SecurityTutorialsSqlMembershipProvider` poskytovatele, výše uvedené značky definují `SecurityTutorialsSqlMembershipProvider` jako výchozího zprostředkovatele (prostřednictvím `defaultProvider` atribut `<membership>` element). Připomínáme, že rozhraní členství může mít víc registrovaných zprostředkovatelů. Protože `AspNetSqlMembershipProvider` je zaregistrovaný jako první poskytovatel v `machine.config`, slouží jako výchozí zprostředkovatel není-li Udáváme jinak.

V současné době aplikace má dva registrovaného zprostředkovatele: `AspNetSqlMembershipProvider` a `SecurityTutorialsSqlMembershipProvider`. Ale před registrací `SecurityTutorialsSqlMembershipProvider` poskytovatele jsme mohli bylo zrušeno zaškrtnutí všeho dříve registrovaných zprostředkovatelů tak, že přidáte [ `<clear />` element](https://msdn.microsoft.com/library/t062y6yc.aspx) bezprostředně před naše `<add>` element. To by smažte `AspNetSqlMembershipProvider` ze seznamu registrovaných zprostředkovatelů, to znamená, že `SecurityTutorialsSqlMembershipProvider` bude pouze registrovaného zprostředkovatele členství. Pokud jsme použili tento postup, pak by potřebujeme k označení `SecurityTutorialsSqlMembershipProvider` jako výchozího zprostředkovatele, protože by bylo pouze registrovaného zprostředkovatele členství. Další informace o používání `<clear />`, naleznete v tématu [použití `<clear />` při přidání poskytovatelů](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Všimněte si, že `SecurityTutorialsSqlMembershipProvider`společnosti `connectionStringName` nastavení odkazy právě přidané `SecurityTutorialsConnectionString` název připojovacího řetězce a že jeho `applicationName` bylo nastaveno na hodnotu SecurityTutorials. Kromě toho `requiresUniqueEmail` nastavení je nastavená na `true`. Všechny ostatní možnosti konfigurace jsou identické s hodnotami v `AspNetSqlMembershipProvider`. Nebojte se provádět žádné změny konfigurace, pokud chcete. Tím, že vyžaduje dva jiné než alfanumerické znaky, místo jednoho nebo zvýšení délky hesla na osm znaků místo sedmidílné série, například může zkracují síly hesla.

> [!NOTE]
> Připomínáme, že členství v rámci umožňuje pro jednoho uživatele úložiště k rozdělení na oddíly napříč více aplikacemi. Zprostředkovatel členství `applicationName` nastavení určuje, jaké aplikace, poskytovatel použije při práci s úložišti uživatele. Je důležité, explicitně nastavit hodnotu `applicationName` nastavení konfigurace, protože pokud `applicationName` není explicitně nastaven, je přiřazená virtuální kořenová cesta webové aplikace za běhu. To funguje dobře tak dlouho, dokud nedojde ke změně virtuální kořenová cesta aplikace, ale pokud přesouváte aplikace do jiného umístění, `applicationName` příliš změní nastavení. Pokud k tomu dojde, bude zprostředkovatel členství začít pracovat s oddíl různé aplikace, než se využívala. Uživatelské účty vytvořené před přesunutí se bude nacházet v jiné aplikaci oddílu a tyto uživatelé už nebudou moct přihlásit k webu. V této věci se podrobněji probírají, naleznete v tématu [vždycky nastavený `applicationName` vlastnost při konfiguraci členství technologie ASP.NET 2.0 a dalších poskytovatelů](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Souhrn

V tuto chvíli máme databáze s nakonfigurovanou aplikační služby (`SecurityTutorials.mdf`) a nakonfigurovali naši webovou aplikaci tak, aby používala rozhraní členství `SecurityTutorialsSqlMembershipProvider` jsme registrovaný poskytovatel. Je tento registrovaný poskytovatel typu `SqlMembershipProvider` a má jeho `connectionStringName` nastavte příslušný připojovací řetězec (`SecurityTutorialsConnectionString`) a jeho `applicationName` explicitně nastavena hodnota.

Nyní jsme připraveni použít rozhraní členství z naší aplikace. V dalším kurzu prozkoumáme vytvoření nové uživatelské účty. Následující, že se podíváme na ověřování uživatelů, autorizaci založené na uživatelích a ukládá Další informace související s uživatelem v databázi.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Vždycky nastavený `applicationName` vlastnost při konfiguraci členství technologie ASP.NET 2.0 a dalších poskytovatelů](https://weblogs.asp.net/scottgu/443634)
- [Konfigurace technologie ASP.NET 2.0 služeb aplikací použít SQL Server 2000 nebo SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Stáhněte si SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Zkoumání ASP.NET 2.0 s členství, role a profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>` – Element pro zprostředkovatele pro členství](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>` – Element](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>` – Element pro členství](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Pomocí `<clear />` při přidání poskytovatele](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Práce přímo se `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video od Pluralsightu na témata, které jsou obsažené v tomto kurzu

- [Principy členství v ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Konfigurace SQL kvůli spolupráci se schématy členství](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Změna nastavení členství ve výchozím schématu členství](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Alicja Maziarz. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](storing-additional-user-information-cs.md)
> [další](creating-user-accounts-vb.md)
