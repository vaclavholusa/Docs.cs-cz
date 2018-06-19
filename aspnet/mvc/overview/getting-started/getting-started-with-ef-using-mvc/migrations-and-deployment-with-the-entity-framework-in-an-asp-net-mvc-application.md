---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Migrace a nasazení pomocí rozhraní Entity Framework v aplikaci ASP.NET MVC nejprve kódu | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 04d393edca0469df140f06a7d083a48aa8f84b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879553"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Nejprve kódu migrace a nasazení pomocí rozhraní Entity Framework v aplikaci ASP.NET MVC
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Pokud spuštění aplikace místně v IIS Express na vašem vývojovém počítači. Chcete-li reálné aplikaci zpřístupnit ostatním uživatelům používat přes Internet, budete muset nasadit do webové poskytovatele hostitelských služeb. V tomto kurzu budete nasazovat aplikace Contoso univerzity do cloudu v Azure.

Tento kurz obsahuje následující části:

- Povolte migrace Code First. Funkce migrace umožňuje změnit datový model a nasazovat změny do produkčního prostředí s aktualizace schématu databáze, aniž by bylo nutné vyřadit a znovu vytvořit databázi.
- Nasazení do Azure. Tento krok je volitelný; Zbývající kurzech můžete pokračovat bez nutnosti nasadili projekt.

Doporučujeme vám, že pomocí procesu průběžnou integraci s zdrojového kódu pro nasazení, ale v tomto kurzu ale nenajdete, tato témata. Další informace najdete v tématu [ovládací prvek zdroje](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) a [průběžnou integraci](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) kapitoly [vytváření reálných cloudových aplikací s Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) elektronických knih.

## <a name="enable-code-first-migrations"></a>Povolení migrace Code First

Při vývoji nových aplikací datový model změní často a pokaždé, když změn modelu, získá synchronizována s databází. Jste nakonfigurovali automaticky vyřadit a znovu vytvořit databázi při každé změně datového modelu Entity Framework. Pokud můžete přidat, odebrat, nebo změňte tříd entit nebo změnit vaše `DbContext` třídy, při příštím spuštění aplikace automaticky odstraní existující databázi, vytvoří novou, který odpovídá modelu a doplňuje s testovacích datech.

Udržování databáze synchronizace s datovým modelem, tato metoda funguje dobře, dokud nasazení aplikace do produkčního prostředí. Když je aplikace spuštěna v produkčním prostředí, je obvykle ukládá data, která chcete zachovat a nechcete ztratit všechno pokaždé, když provedete změny, jako je například přidávání nové sloupce. [Migrace Code First](https://msdn.microsoft.com/data/jj591621) funkce tento problém řeší tím, že umožňuje Code First aktualizovat schéma databáze místo vyřadit a znovu vytvořit databázi. V tomto kurzu budete nasazovat aplikace a příprava na který budete povolit migrace.

1. Zakázat inicializátoru, kterou jste vytvořili dříve při psaní komentářů nebo odstranění `contexts` element, který jste přidali do souboru Web.config aplikace.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Také v aplikaci *Web.config* souboru, změňte název databáze v připojovacím řetězci ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Tato změna nastaví projekt tak, aby první migrací vytvoří novou databázi. Tato akce není povinná, ale se zobrazí později proto je vhodné.
3. Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. Na `PM>` řádku zadejte následující příkazy:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![příkaz enable-migrations se](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations` Příkaz vytvoří *migrace* složky v projektu ContosoUniversity a uloží je v této složce *Configuration.cs* soubor, který můžete upravit konfigurace migrací.

    (Pokud provedena předchozí krok, který přesměruje, budete muset změnit název databáze, bude najít existující databázi a automaticky se migrace `add-migration` příkaz. To je v pořádku, právě znamená, že před nasazením databáze nebude spustit test migrace kódu. Později při spuštění `update-database` příkaz není provedena žádná akce vzhledem k tomu, že bude databáze již existovat.)

    ![Složku migrations](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Inicializátor třídu, která jste viděli dříve, jako `Configuration` třída obsahuje `Seed` metoda.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Účelem [počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda je umožnit vložit nebo aktualizovat testovací data po Code First vytvoří nebo aktualizuje databázi. Metoda je volána při vytvoření databáze, a při každé aktualizaci schématu databáze po změně datového modelu.

### <a name="set-up-the-seed-method"></a>Nastavit Seed – metoda

Když jsou odstranit a znovu vytvořit databázi u každé změny datového modelu, můžete použít třídu inicializátoru `Seed` dojde ke ztrátě metoda vložit testovací data, protože po každé změně modelu je odstranit databázi a v testovacích datech. Pomocí migrace Code First, testovací data se uchovávají po provedení změn databáze, takže včetně testovací data v [počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda není obvykle nutné. Ve skutečnosti, které nechcete `Seed` metoda vložit testovací data, pokud budete používat migrace pro nasazení databáze do produkčního prostředí, protože `Seed` metoda bude spuštěna v produkčním prostředí. V takovém případě má `Seed` metoda vložit do databáze pouze data, která je nutné v produkčním prostředí. Například můžete zahrnout názvy skutečné oddělení v databázi `Department` tabulky, až aplikace k dispozici v produkčním prostředí.

V tomto kurzu budete používat migrace pro nasazení, ale vaše `Seed` metoda bude přesto vložit testovací data pro snazší zjistit, jak funguje funkce aplikace, aniž by bylo nutné ručně vložit velké množství dat.

1. Nahraďte obsah *Configuration.cs* soubor s následující kód, který načte testovací data do nové databáze. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [Počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda přebírá objekt kontextu databáze jako vstupní parametr a kód v metodě používá tento objekt k přidání nové entity do databáze. Ke každému typu entity kód vytvoří kolekci nové entity, se přidají do příslušné [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) vlastnost a uloží změny do databáze. Není nutné volat [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda po každé skupiny entit, jako je na jednom místě, ale umožňuje učinit vyhledejte zdroj problému, pokud dojde k výjimce při kód je zápis do databáze.

    Některé příkazy, který vkládá data pomocí [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda o provedení operace "upsert". Protože `Seed` metoda spuštěna při každém spuštění `update-database` příkazu, obvykle po každé migraci dat, nelze právě vložit, protože řádky, které se pokoušíte přidat bude po první migrace, která vytváří databázi již existovat. Operace "upsert", zabraňuje chyb, které by mohlo dojít, pokud se pokusíte vložit řádek, který již existuje, ale ***přepsání*** všechny změny dat, která může provedení při testování aplikace. Pro testovací data v některé tabulky nemusí Chcete to tak proběhlo: v některých případech při změně dat při testování vaše změny zůstat po aktualizace databáze. V takovém případě budete chtít provést operace podmíněného insert: Vložit řádek pouze v případě, že ještě neexistuje. Metoda počáteční hodnoty pomocí obou přístupů.

    První parametr předaný [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda určuje vlastnost, která má použít pro kontrolu, pokud řádek již existuje. Pro testovací data studenty, který zadáte `LastName` vlastnost lze použít pro tento účel, protože každý příjmení v seznamu je jedinečný:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Tento kód předpokládá, že poslední názvy jsou jedinečné. Pokud chcete ručně přidat student s duplicitním názvem poslední, získáte následující výjimka při příštím provedení migrace.

    Sekvence obsahuje více než jeden element.

    Informace o způsobu zpracování redundantních dat, jako je například dvou studentů s názvem "Alexander Carsonovy" najdete v tématu [první financování a ladění Entity Framework (EF) databází](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Ricka Andersona. Další informace o `AddOrUpdate` metodu, najdete v části [postará s EF 4.3 AddOrUpdate metoda](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

    Kód, který vytvoří `Enrollment` entity předpokládá, že máte `ID` hodnota v entity v `students` kolekce, i když tuto vlastnost nebyla nastavena v kódu, který vytvoří kolekci.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Můžete použít `ID` vlastnost zde protože `ID` hodnota při volání `SaveChanges` pro `students` kolekce. EF automaticky získá hodnotu primárního klíče, když se vloží entitu do databáze, a aktualizuje `ID` vlastnost entity v paměti.

    Kód, který přidá všechny `Enrollment` entity, která má `Enrollments` sady entit nepoužívá `AddOrUpdate` metoda. Zkontroluje, zda entita již existuje a vloží entitu, pokud neexistuje. Tento přístup se zachovat změny, které můžete provést úrovni registrace pomocí uživatelského rozhraní aplikace. Kód prochází každý člen `Enrollment` [seznamu](https://msdn.microsoft.com/library/6sh2ey19.aspx) a pokud registrace nebyla nalezena v databázi, přidá zápisu do databáze. Při prvním aktualizaci databáze, databáze se bude prázdný, proto přidá každou registrace.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Sestavte projekt.

### <a name="execute-the-first-migration"></a>Provést první migrace

Při provedení `add-migration` příkaz migrace generovaného kódu, který by vytvoření databáze od začátku. Tento kód je také v *migrace* složku, v souboru s názvem  *&lt;časové razítko&gt;\_InitialCreate.cs*. `Up` Metodu `InitialCreate` třída vytvoří databázové tabulky, které odpovídají sady dat modelu entity a `Down` je metoda odstraní.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Migrace volání `Up` metody k implementaci změny modelu dat pro migraci. Když zadáte příkaz k vrácení aktualizace, migrace volání `Down` metoda.

Toto je počáteční migrace, která byla vytvořena, když jste zadali `add-migration InitialCreate` příkaz. Parametr (`InitialCreate` v příkladu) se používá pro soubor název a může být všechno; obvykle zvolíte slovo nebo frázi, která shrnuje, co probíhá při migraci. Například můžete třeba pojmenovat novější migrace &quot;AddDepartmentTable&quot;.

Pokud jste vytvořili počáteční migrace, když databáze již existuje, se vygeneruje kód pro vytvoření databáze, ale nemá spustit, protože databáze již odpovídá datový model. Když nasadíte aplikaci do jiného prostředí, kde databáze ještě neexistuje, tento kód spustí k vytvoření databáze, proto je vhodné se nejdřív otestovat. To je důvod, proč jste změnili název databáze v připojovacím řetězci dříve – tak, aby migrace můžete vytvořit novou od začátku.

1. V **Konzola správce balíčků** okno, zadejte následující příkaz:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database` Příkaz spustí `Up` metodu pro vytvoření databáze a pak se spustí `Seed` metoda k naplnění databáze. Stejný postup se spustí automaticky v produkčním prostředí poté, co nasadíte aplikaci, jak uvidíte v následující části.
2. Použití **Průzkumníka serveru** kontrolovat databázi, stejně jako v prvním kurzu a spuštění aplikace k ověření, že všechno stále funguje stejně jako předtím.

## <a name="deploy-to-azure"></a>Nasazení do Azure

Pokud spuštění aplikace místně v IIS Express na vašem vývojovém počítači. Chcete-li k dispozici pro ostatní uživatelé používat přes Internet, musíte ji nasadit do webové poskytovatele hostitelských služeb. V této části kurzu budete ji nasadit do Azure. Tato část je volitelná. Můžete to přeskočit a pokračujte v následujícím kurzu, nebo můžete přizpůsobit pokyny v této části jiného poskytovatele hostitelských služeb podle svého výběru.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Pomocí migrace Code First pro nasazení databáze

Pro nasazení databáze používáte migrace Code First. Když vytvoříte profil publikování, který použijete ke konfiguraci nastavení pro nasazení ze sady Visual Studio, vyberte zaškrtávací políčko s názvem **aktualizace databáze**. Toto nastavení způsobí, že proces nasazení pro automatickou konfiguraci aplikace *Web.config* souboru na cílovém serveru tak, aby používala Code First `MigrateDatabaseToLatestVersion` inicializátoru třídy.

Visual Studio nic se neděje s databází během procesu nasazení při jeho kopíruje projektu na cílový server. Když spustíte nasazené aplikace a přistupuje k databázi po nasazení poprvé, Code First kontroluje, pokud databáze odpovídá datový model. Pokud došlo k neshodě, Code First automaticky vytvoří databázi (pokud ještě neexistuje) nebo aktualizace schématu databáze na nejnovější verzi (Pokud je databáze existuje, ale neodpovídají modelu). Pokud aplikace implementuje migrace `Seed` metoda, metoda spustí po vytvoření databáze nebo schéma se aktualizuje.

Vaše migrace `Seed` metoda vloží testovacích datech. Pokud byly nasazení v produkčním prostředí, je třeba změnit `Seed` metody, které se vloží pouze data, která má být vložen do provozní databáze. V aktuální datového modelu můžete třeba skutečné kurzy ale fiktivních studenti, kteří mají v databázi vývoj. Můžete napsat `Seed` má metoda v vývoj i zatížení a potom komentář fiktivních studenty před nasazením do produkčního prostředí. Nebo můžete napsat `Seed` metoda načíst pouze kurzy, a zadejte fiktivních studenty v databázi testovací ručně pomocí uživatelského rozhraní aplikace.

### <a name="get-an-azure-account"></a>Získat účet Azure

Budete potřebovat účet Azure. Pokud jste ještě nemáte, ale máte předplatné sady Visual Studio, můžete [aktivovat výhody předplatné](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Jinak můžete vytvořit Bezplatný zkušební účet si během několika minut. Podrobnosti najdete v tématu [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Vytvoření webu a databázi SQL v Azure

Webové aplikace v Azure se spustí ve sdíleném hostování prostředí, což znamená, že běží na virtuálních počítačích (VM), které jsou sdíleny s jinými klienty Azure. Sdílené hostování prostředí je nízkonákladové způsob, jak začít pracovat v cloudu. Později webového provozu se zvyšuje, aplikace můžete škálovat podle potřeby spuštěním na vyhrazených virtuálních počítačích. Další informace o cenách možnosti pro službu Azure App Service, přečtěte si dokumentaci na [dokumentace Azure](https://azure.microsoft.com/pricing/details/app-service/)

Databáze budete nasazovat do Azure SQL Database. Databáze SQL je služba relační databáze založené na cloudu, která je založen na technologiích systému SQL Server. Nástroje a aplikace, které pracují s SQL serverem také pracovat s databází SQL.

1. V [portálu pro správu Azure](https://portal.azure.com), klikněte na tlačítko **nový** v levé kartě klikněte na **zobrazit všechny** v novém okně a pak klikněte na tlačítko **webové aplikace a SQL** v **Webové** části a v neposlední řadě **vytvořit**.

    ![Tlačítko Nový na portálu pro správu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   **Novou webovou aplikaci a SQL - vytvořit** otevře se průvodce.

2. V okně zadejte řetězec ve **název aplikace** pole, které chcete použít jako jedinečnou adresu URL pro vaši aplikaci. Úplná adresa URL bude obsahovat co zadáte plus výchozí doménu služby Azure App Services (. azurewebsites.net). Pokud **název aplikace** už používá, průvodce bude oznámíme vám to červený *název aplikace není k dispozici* zprávy. Pokud **název aplikace** je k dispozici, zobrazí se zeleného zaškrtnutí.

    ![Vytvoření pomocí připojení databáze na portálu pro správu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. V **předplatné** rozevíracího seznamu, vyberte předplatné Azure, ve kterém chcete **služby App Service** umístíte.

4. V **skupiny prostředků** textového pole, zvolte skupinu prostředků nebo vytvořte novou. Toto nastavení určuje, které datového centra, webový server se spustí v. Další informace o skupinách prostředků naleznete v dokumentaci na [dokumentace Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Vytvořte novou **plán služby App Service** kliknutím *části služby App Service*, **vytvořit nový**a vyplňte **plán služby App Service** (může být stejný název jako App Service), **umístění**, a **cenová úroveň** (je bezplatnou možností).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Klikněte **databáze SQL**a zvolte *vytvořit nový* nebo vyberte existující databázi

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. V **název** pole, zadejte název pro vaši databázi.
8. Klikněte **cílový Server** vyberte **vytvořit nový server**. Případně pokud jste dříve vytvořili serveru, můžete vybrat tento server ze seznamu dostupných serverů.
9. Zvolte **cenová úroveň** zvolte *volné*. Pokud je nutné provést další prostředky, je možné škálovat databázi kdykoli. Další informace o cenách SQL Azure, přečtěte si dokumentaci na [dokumentace Azure](https://azure.microsoft.com/pricing/details/sql-database/).
10. Upravit [kolace](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) podle potřeby.
11. Zadejte správce **uživatelské jméno správce SQL** a **heslo správce SQL**. Pokud jste vybrali **novou databázi SQL serveru**nezadáváte existující jméno a heslo, zadáváte nové jméno a heslo, které teď definujete pro pozdější použití při přístupu k databázi. Pokud jste vybrali serveru, který jste vytvořili dříve, budete zadejte přihlašovací údaje pro tento server.
12. Kolekce telemetrie lze povolit pro službu App Service pomocí Application Insights. Application Insights s malým množstvím konfigurací shromažďuje cenné událostí, výjimky, závislostí, žádost a informace o trasování. Další informace o Application Insights, začněte [dokumentace Azure](https://azure.microsoft.com/services/application-insights/).
13. Klikněte na tlačítko **vytvořit** v dolní části okna znamenat, že budete hotovi.
  
    Vrátí portálu pro správu na stránku řídicí panely a **oznámení** vytvoření webu se zobrazí okno v horní části stránky. Po chvíli (obvykle během méně než minuty) bude oznámení, že nasazení bylo úspěšné. Na navigačním panelu na levé straně nové **služby App Service** se zobrazí v *App Services* části a nové **SQL Database** se zobrazí v *databáze SQL*  části.

### <a name="deploy-the-application-to-azure"></a>Nasaďte aplikaci do Azure

1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **publikovat** v místní nabídce.
  
    ![Publikování v kontextové nabídce projektu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. V **profil** kartě **Publikovat Web** průvodce, klikněte na tlačítko **Microsoft Azure App Service**.
  
    ![Import nastavení publikování](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Pokud jste ještě přidali dříve vašeho předplatného Azure v sadě Visual Studio, proveďte kroky na obrazovce. Tyto kroky aktivují Visual Studio k připojení k předplatnému Azure, který seznam **App Services** bude obsahovat vašeho webu.
 
4. Vyberte **předplatné** jste přidali do, pak App Service **plán služby App Service** složky App Service je součástí, a nakonec **služby App Service** samotné následuje **Ok**.

    ![Vyberte aplikační služby](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Po nakonfigurovaný profil, **připojení** se zobrazí karta. Klikněte na tlačítko **ověřit připojení** a ujistěte se, zda jsou správné nastavení

    ![Ověření připojení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Při připojení byl ověřen, je vedle zobrazí zelená značka zaškrtnutí **ověřit připojení** tlačítko. Klikněte na tlačítko **Další**.
  
    ![Úspěšně ověřená připojení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Otevřete **vzdáleného připojovací řetězec** rozevíracím seznamu v části **SchoolContext** a vyberte připojovací řetězec pro databázi, který jste vytvořili.
8. Vyberte **aktualizace databáze**.

    ![Karta nastavení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Toto nastavení způsobí, že proces nasazení pro automatickou konfiguraci aplikace *Web.config* souboru na cílovém serveru tak, aby používala Code First `MigrateDatabaseToLatestVersion` inicializátoru třídy.
9. Klikněte na tlačítko **Další**.
10. V **Preview** , klikněte na **spustit Náhled**.
  
    ![Tlačítko StartPreview na kartě Preview](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    Na kartě zobrazí seznam souborů, které se zkopírují na serveru. Zobrazení náhledu není vyžadován pro publikování aplikace, ale je užitečné funkce znát. V takovém případě nemusíte dělat nic s seznam souborů, který se zobrazí. Pouze soubory, které se změnily při příštím nasadit tuto aplikaci, bude v tomto seznamu.
    ![Výstup souboru StartPreview](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Klikněte na tlačítko **publikování**.
    Visual Studio spustí proces kopírování souborů na Azure server.
12. **Výstup** okno zobrazuje, jaké akce nasazení byly provedeny a hlásí úspěšné dokončení nasazení.
  
    ![Výstup – okno hlášením úspěšného nasazení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Po úspěšném nasazení na adresu URL nasazené webové stránky automaticky otevře výchozí prohlížeč.
    Aplikace, kterou jste vytvořili je nyní spuštěna v cloudu. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

V tomto okamžiku vaší *SchoolContext* databáze byla vytvořena ve službě Azure SQL Database, protože jste vybrali **spustit migrace Code First (spuštěno při spuštění aplikace)**. *Web.config* změnil soubor na nasazené webu tak, aby [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializátoru spustí při prvním kód čte nebo zapisuje data v databázi (která se stalo Pokud jste vybrali **studenty** kartě):

![](https://asp.net/media/4367421/mig.png)

Proces nasazení taky vytvořit nový připojovací řetězec *(SchoolContext\_DatabasePublish*) pro migrace Code First pro aktualizace schématu databáze a synchronizace replik indexů databáze.

![Database_Publish připojovací řetězec](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Můžete najít soubor Web.config na vašem počítači v nasazené verzi *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Dostanete nasazené *Web.config* samotném souboru pomocí protokolu FTP. Pokyny najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Postupujte podle pokynů, které začínají na "pomocí nástroje FTP, budete potřebovat tři věci: adresy URL FTP, uživatelské jméno a heslo."

> [!NOTE]
> Webové aplikace neimplementuje zabezpečení, takže každý, kdo najde adresu URL můžete změnit data. Pokyny k zabezpečení webu, najdete v části [nasazení aplikace zabezpečené ASP.NET MVC s členství, OAuth a databáze SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Můžete zabránit jinými lidmi pomocí webu pomocí portálu pro správu Azure nebo **Průzkumníka serveru** v sadě Visual Studio k zastavení webu.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Migrace pokročilé scénáře

Pokud nasazení databáze spuštěním migrace automaticky, jak je znázorněno v tomto kurzu, a provádíte nasazení webu, který běží na několika serverech, může dojít, více serverů pokusu o spuštění migrace ve stejnou dobu. Migrace jsou atomic, takže pokud dva servery se pokusí spustit stejné migraci, jeden bude úspěšné a dalších selže (za předpokladu, že operace, které nelze provádět dvakrát). V tomto scénáři Pokud chcete, aby se zabránilo těchto problémů, můžete volat migrace ručně a nastavit vlastní kód tak, aby ho se dělá jenom jednou. Další informace najdete v tématu [spuštěná a skriptování migrace z kódu](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) na blogu Rowan Lukeš a [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (pro provádění migrace z příkazového řádku) na webu MSDN.

Informace o dalších scénářů migrace najdete v tématu [migrace záznam dění na monitoru řady](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>První inicializátory kódu

V části nasazení jste viděli [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializátoru používá. Kód nejprve taky poskytuje další inicializátory, včetně [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (výchozí), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (který jste použili dříve) a [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Inicializátoru může být užitečná pro nastavení podmínky pro testování částí. Můžete taky napsat vlastní inicializátory a můžete volat inicializátoru explicitně Pokud nechcete čekat, až aplikace čte z nebo zapisuje do databáze. V době, kdy probíhá zápis v tomto kurzu v listopadu 2013 můžete použít pouze inicializátory vytvořit a DropCreate dříve než povolíte migrace. Rozhraní Entity Framework tým pracuje na vytváření těchto inicializátory použitelné s migrací také.

Další informace o inicializátory najdete v tématu [Principy inicializátory databáze v Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) a kapitoly 6 knihy [programovací rozhraní Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) podle Julie Lerman a Rowan Lukeš.

## <a name="summary"></a>Souhrn

V tomto kurzu jste se seznámili povolení migrace a nasazení aplikace. V dalším kurzu brzy prohlížení pokročilejší témata rozšířením datový model.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Předchozí](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [další](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
