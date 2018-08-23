---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Kód nejprve Migration a nasazení s Entity Framework v aplikaci ASP.NET MVC | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 5 pomocí sady Visual Studio a Entity Framework 6 Code First...
ms.author: riande
ms.date: 11/07/2014
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4c1f852bab5e8f77b35239c356d11b5058cbdaef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752646"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Kód nejprve Migration a nasazení s Entity Framework v aplikaci ASP.NET MVC
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stahovat PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 5 pomocí sady Visual Studio 2013 a Entity Framework 6 Code First. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Zatím aplikace je spuštěné místně v rámci služby IIS Express na vašem vývojovém počítači. Aplikace skutečný zpřístupnit ostatním uživatelům používat přes Internet, musíte nasadit na web poskytovatele hostitelských služeb. V tomto kurzu nasadíte aplikaci Contoso University do cloudu v Azure.

Tento kurz obsahuje následující části:

- Povolení migrace Code First. Funkce migrace umožňuje změnit datový model a vaše změny nasadí do produkčního prostředí o aktualizaci schématu databáze, aniž by bylo nutné vyřadit a znovu vytvořit databázi.
- Nasazení do Azure. Tento krok je nepovinný; Zbývající kurzech můžete pokračovat bez nutnosti nasazení projektu.

Doporučujeme použít proces průběžné integrace se správou zdrojového kódu pro nasazení, ale v tomto kurzu ale nenajdete, tato témata. Další informace najdete v tématu [správy zdrojového kódu](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) a [kontinuální integrace](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) kapitoly [vytváření skutečných cloudových aplikací s využitím Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction) e kniha.

## <a name="enable-code-first-migrations"></a>Povolení migrace Code First

Při vývoji nových aplikací, datového modelu mění často a pokaždé, když změny modelu, získá synchronizován s databází. Nakonfigurovali jste automaticky vyřadit a znovu vytvořit databázi pokaždé, když změníte datový model Entity Framework. Při přidání, odebrání, nebo změnit tříd entit nebo změnit váš `DbContext` třídy, při příštím spuštění aplikace automaticky odstraní existující databázi, vytvoří nový, který odpovídá modelu a nasazení se nasazuje s testovací data.

Tato metoda zachování databáze synchronizované s datovým modelem funguje dobře, dokud nasadit aplikaci do produkčního prostředí. Když aplikace běží v produkčním prostředí, je obvykle ukládá data, která chcete zachovat, a nechcete ztratit všechno, co pokaždé, když provedete změny, např. přidejte nový sloupec. [Migrace Code First](https://msdn.microsoft.com/data/jj591621) funkce tento problém řeší tím, že Code First pro aktualizaci schématu databáze místo vyřadit a znovu vytvořit databázi. V tomto kurzu budete nasazovat aplikaci a příprava, který vám umožní migraci.

1. Zakázat inicializátoru, který jste nastavili dříve okomentováním odpovídajícího nebo odstraněním `contexts` element, který jste přidali do souboru Web.config aplikace.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Také v aplikaci *Web.config* změňte název databáze v připojovacím řetězci ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Tato změna nastaví projekt tak, aby první migrací vytvoří novou databázi. Tato akce není povinná, ale zobrazí se vám později důvod, proč je vhodné.
3. Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. Na `PM>` řádku zadejte následující příkazy:

    `enable-migrations`  
    `add-migration InitialCreate`

    ![příkaz povolení migrace](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations` Příkaz vytvoří *migrace* složky v projektu ContosoUniversity a umístí v této složce *Configuration.cs* souboru, který můžete upravit konfiguraci migrace.

    (Pokud předchozí krok, který nasměruje můžete změnit název databáze, migrace se najít existující databázi a automaticky provést `add-migration` příkazu. To je v pořádku, to pouze znamená, že před nasazením databáze nebude spuštění testu migrace kódu. Později při spuštění `update-database` příkaz nic se nestane, protože databáze bude existovat.)

    ![Migrace složky](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Jako inicializátor třídy, která jste viděli dříve, `Configuration` třída zahrnuje `Seed` metoda.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    Účelem [počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodou je vám umožní vložit nebo aktualizovat data testu po Code First vytvoří nebo aktualizuje databázi. Metoda je volána při vytvoření databáze a pokaždé, když je schéma databáze se aktualizuje po změně datového modelu.

### <a name="set-up-the-seed-method"></a>Nastavit Seed – metoda

Když přetahujete a opětovné vytvoření databáze u každé změny datového modelu, můžete použít inicializátor třídy `Seed` metoda vložit testovací data, protože po každé změně modelu databázi vyřadit a všechny testovací data se ztratí. Pomocí migrace Code First, testovací data se uchovávají po provedení změn databáze, takže včetně testovací data v [počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodu není obvykle nutné. Ve skutečnosti nechcete, aby `Seed` metoda k vložení dat testu, pokud budete používat migrace nasazení databáze do produkčního prostředí, protože `Seed` metoda bude spuštěna v produkčním prostředí. V takovém případě má `Seed` metoda k vložení do databáze pouze data, která budete potřebovat v produkčním prostředí. Například může být vhodné zahrnout názvy skutečné oddělení v databázi `Department` tabulky, když se aplikace zpřístupní v produkčním prostředí.

Pro účely tohoto kurzu budete používat migrace pro nasazení, ale vaše `Seed` metoda bude přesto vložit testovací data pro snazší zjistit, jak funguje funkčnost aplikace, aniž byste museli ručně vložit velké množství dat.

1. Nahraďte obsah *Configuration.cs* souboru následujícím kódem, který načte testovací data do nové databáze. 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [Počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda přijímá jako vstupní parametr objekt kontextu databáze, a kód v metodě používá tento objekt k přidání nové entity do databáze. Pro každý typ entity kód vytvoří kolekci nové entity, se přidají do příslušné [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) vlastnost a uloží změny do databáze. Není nutné volat [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda po každé skupiny entit, jako se zde provádí, ale způsobem, který pomáhá při hledání příčiny problému, pokud dojde k výjimce, zatímco kód zapisuje do databáze.

    Některé příkazy, které vkládají data používají [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody k provedení operace "upsert". Protože `Seed` metoda se spustí pokaždé, když spustíte `update-database` příkazu, obvykle po každou migraci dat, nemůže právě vložit, protože se snažíte přidat řádky už budou tam po první migrace, který vytvoří databázi. Operace "upsert", zabraňuje chybám, které by mohlo dojít, pokud se pokusíte vložit řádek, který již existuje, ale ***přepíše*** všechny změny dat, který může mít provedené při testování aplikace. Pomocí testovacích dat v některých tabulkách nechcete provést: v některých případech při změně dat při testování chcete své změny zůstat po aktualizacích databáze. V takovém případě ji chcete vložit podmíněné operace: Vložit řádek jenom v případě, že ještě neexistuje. Seed – metoda používá oba přístupy.

    První parametr předána [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody určuje vlastnost, která má použít ke kontrole, jestli řádek již existuje. Pro testovací data studentů, které poskytujete `LastName` vlastnost lze použít pro tento účel, protože je jedinečný název každé poslední v seznamu:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Tento kód předpokládá, že poslední názvy musí být jedinečné. Pokud chcete ručně přidat studenta s duplicitním názvem poslední, zobrazí se následující výjimka při příštím provedení migrace.

    Posloupnost obsahuje více než jeden prvek.

    Informace o tom, jak zpracování redundantních dat, jako je například dvou studentů s názvem "Alexander Carsonovy" najdete v tématu [financování a ladění Entity Framework (EF) databází](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Ricka Andersona. Další informace o `AddOrUpdate` metodu, najdete v článku [postará metodou AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

    Kód, který vytvoří `Enrollment` entity předpokládá, že máte `ID` hodnotu entit v `students` kolekcí, i když jste nenastavili tuto vlastnost v kódu, který vytvoří kolekci.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Můžete použít `ID` vlastnost zde protože `ID` hodnotu při volání `SaveChanges` pro `students` kolekce. EF automaticky získá hodnotu primárního klíče, když se vloží entitu do databáze, a aktualizuje `ID` vlastnost entity v paměti.

    Kód, který přidá všechny `Enrollment` entita, která má `Enrollments` nepoužívá sadu entit `AddOrUpdate` metoda. Zkontroluje, jestli entita již existuje a vloží entitu, pokud neexistuje. Tento přístup se zachovat změny provedené na podnikové úrovni registraci pomocí aplikace uživatelského rozhraní. Kód prochází každý člen `Enrollment` [seznamu](https://msdn.microsoft.com/library/6sh2ey19.aspx) a pokud registrace nebyl nalezen v databázi, přidá přihlášení k databázi. Při prvním aktualizaci databáze, databáze se bude prázdný, tak přidá každý registrace.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. Sestavte projekt.

### <a name="execute-the-first-migration"></a>Spuštění první migrace

Při spouštění `add-migration` příkazu migrace vygeneruje kód, který by vytvořila databázi úplně od začátku. Tento kód je také v *migrace* složku, v souboru s názvem  *&lt;časové razítko&gt;\_InitialCreate.cs*. `Up` Metodu `InitialCreate` třída vytvoří databázové tabulky, které odpovídají sady entit datového modelu a `Down` je metoda odstraní.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Migrace volání `Up` metody k implementaci změn datových modelů pro migraci. Po zadání příkazu vrácení zpět aktualizace, volání migrace `Down` metody.

Toto je počáteční migraci, která byla vytvořena, když jste zadali `add-migration InitialCreate` příkazu. Parametr (`InitialCreate` v příkladu) se používá pro soubor pojmenujte a může být cokoliv, co chcete; obvykle zvolíte slovo nebo slovní spojení, které shrnuje, co se provádí v migraci. Můžete například pojmenovat pozdější migraci &quot;AddDepartmentTable&quot;.

Pokud jste vytvořili počáteční migraci databáze již existuje, vygeneruje kód pro vytvoření databáze, ale nemusí spustit, protože databáze již odpovídá datového modelu. Když nasadíte aplikaci do jiného prostředí, kde databáze neexistuje ale tento kód spustí k vytvoření databáze, proto je vhodné se nejdřív otestovat. To je důvod, proč jste změnili název databáze v připojovacím řetězci dříve – tak, aby migrace můžete vytvořit nový úplně od začátku.

1. V **Konzola správce balíčků** okno, zadejte následující příkaz:

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database` Příkaz spustí `Up` spustí metodu pro vytvoření databáze a pak ho `Seed` metoda k naplnění databáze. Stejný postup se spustí automaticky v produkčním prostředí po nasazení aplikace, jak uvidíte v následující části.
2. Použití **Průzkumníka serveru** ke kontrole databáze, stejně jako v prvním kurzu a spuštění aplikace pro ověření, že vše stále funguje stejně jako předtím.

## <a name="deploy-to-azure"></a>Nasazení do Azure

Zatím aplikace je spuštěné místně v rámci služby IIS Express na vašem vývojovém počítači. Chcete-li k dispozici pro ostatní použití přes Internet, musíte nasadit na web poskytovatele hostitelských služeb. V této části kurzu nasadíte ho do Azure. Tato část je volitelná. Můžete to přeskočit a pokračovat v následujícím kurzu, nebo můžete si přizpůsobit pokyny v této části pro jiného poskytovatele hostitelských služeb podle vašeho výběru.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Pomocí migrace Code First pro nasazení databáze

Nasazení databáze pomocí migrace Code First. Když vytvoříte profil publikování, který použijete ke konfiguraci nastavení pro nasazení ze sady Visual Studio, vyberte zaškrtávací políčko s názvem **aktualizace databáze**. Toto nastavení způsobí, že proces nasazení pro automatickou konfiguraci aplikace *Web.config* souboru na cílovém serveru tak, aby používala Code First `MigrateDatabaseToLatestVersion` inicializátor třídy.

Visual Studio nebude dělat nic s databází během procesu nasazení, zatímco kopíruje váš projekt na cílový server. Při spuštění aplikace nasazené a přistupuje k databázi po nasazení poprvé, Code First zkontroluje, jestli databáze se shoduje s datovým modelem. Pokud dojde k neshodě, Code First automaticky vytvoří databáze (pokud ještě neexistuje) nebo aktualizuje schéma databáze na nejnovější verzi (Pokud je databáze existuje, ale neodpovídá modelu). Pokud aplikace implementuje migrace `Seed` metody, metoda spustí po vytvoření databáze nebo schéma se aktualizuje.

Vaše migrace `Seed` metoda vloží testovací data. Pokud se nasazení do produkčního prostředí, je třeba změnit `Seed` metodu tak, že pouze vkládá nějaká data, která má být vložen do provozní databáze. Aktuální model dat můžete například mít skutečné kurzů, ale fiktivní studenty v databázi vývoj. Můžete napsat `Seed` má metoda načíst i při vývoji a poté komentář fiktivní studenty, před nasazením do produkčního prostředí. Nebo můžete napsat `Seed` má metoda načíst pouze kurzů a zadejte fiktivní studenty v testovací databázi ručně pomocí uživatelského rozhraní aplikace.

### <a name="get-an-azure-account"></a>Vytvoření účtu Azure

Budete potřebovat účet Azure. Pokud ho ještě nemáte, ale máte předplatné sady Visual Studio, můžete si [aktivovat výhody předplatného](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). V opačném případě můžete vytvořit bezplatného zkušebního účtu stačí pár minut. Podrobnosti najdete v tématu [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Vytvoření webu a SQL database v Azure

Webové aplikace v Azure se spustí ve sdíleném hostování prostředí, což znamená, že běží na virtuálních počítačích (VM), které jsou sdíleny s jinými klienty Azure. Sdílené hostitelského prostředí je s nízkými náklady způsob, jak začít pracovat v cloudu. Později hodnota se zvyšuje webový provoz, aplikace můžete škálovat podle potřeby spouští na vyhrazených virtuálních počítačích. Další informace o cenové možnosti pro službu Azure App Service, přečtěte si dokumentaci na [dokumentace Azure](https://azure.microsoft.com/pricing/details/app-service/)

Budete nasazovat databázi do Azure SQL Database. SQL Database je služba relační databáze založené na cloudu, která je založená na technologiích systému SQL Server. Nástroje a aplikace, které fungují s SQL serverem se také pracovat s SQL Database.

1. V [portálu pro správu Azure](https://portal.azure.com), klikněte na tlačítko **nový** na levé kartě klikněte **zobrazit všechny** nové okno a pak klikněte na **webové aplikace a SQL** v **Webové** oddílu a nakonec **vytvořit**.

    ![Tlačítka Nový v portálu pro správu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   **Novou webovou aplikaci a SQL - vytvořit** otevře se průvodce.

2. V okně zadejte řetězec **název aplikace** pole použít jako jedinečná adresa URL pro vaši aplikaci. Úplná adresa URL bude obsahovat co zde zadáte plus výchozí doménu služby Azure App Services (. azurewebsites.net). Pokud **název aplikace** se už používá, Průvodce vás upozorní na to červeným *název aplikace není k dispozici* zprávy. Pokud **název aplikace** je k dispozici, zobrazí se zelené zaškrtnutí.

    ![Vytvoření s odkazem databáze na portálu pro správu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. V **předplatné** rozevíracím seznamu vyberte předplatné Azure, ve kterém chcete **služby App Service** uložená.

4. V **skupiny prostředků** textového pole, vyberte skupinu prostředků nebo vytvořte novou. Toto nastavení určuje datové centrum, kterého webu se spustí v. Další informace o skupinách prostředků najdete v dokumentaci na [dokumentace Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
5. Vytvořte nový **plán služby App Service** kliknutím *oddílu služby App Service*, **vytvořit nový**a vyplňte **plán služby App Service** (může mít stejný název jako App Service), **umístění**, a **cenová úroveň** (je bezplatnou možností).

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. Klikněte na tlačítko **SQL Database**a zvolte *vytvořit nový* nebo vyberte stávající databázi

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. V **název** pole, zadejte název pro vaši databázi.
8. Klikněte na tlačítko **cílový Server** vyberte **vytvořit nový server**. Dříve vytvořený server, je můžete vybrat tento server ze seznamu dostupných serverů.
9. Zvolte **cenová úroveň** zvolte *Free*. Pokud jsou potřeba další prostředky, je možné škálovat databáze v čase. Další informace o cenách pro Azure SQL, přečtěte si dokumentaci na [dokumentace Azure](https://azure.microsoft.com/pricing/details/sql-database/).
10. Upravit [kolace](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support) podle potřeby.
11. Zadejte správce **uživatelské jméno správce SQL** a **heslo správce SQL**. Pokud jste vybrali **nová databáze SQL serveru**, tak nezadáváte existující jméno a heslo, ale nové jméno a heslo, které teď definujete pro pozdější použití při přístupu k databázi. Pokud jste vybrali serveru, který jste předtím vytvořili, zadáte přihlašovací údaje pro tento server.
12. Shromažďování telemetrie je možné povolit pro službu App Service pomocí služby Application Insights. Application Insights s málo konfigurace shromažďuje cenné událost, výjimky, závislosti, žádosti a informace o trasování. Další informace o Application Insights, pustit do práce [dokumentace Azure](https://azure.microsoft.com/services/application-insights/).
13. Klikněte na tlačítko **vytvořit** v dolní části okna k označení, že budete hotovi.
  
    Na portálu pro správu vrátí na stránku řídicích panelů a **oznámení** v horní části stránky okno Vytvoření webu. Po chvíli (obvykle během méně než minutu) budou se oznámení, že nasazení proběhlo úspěšně. Na navigačním panelu na levé straně nové **služby App Service** se zobrazí v *App Services* oddíl a nový **SQL Database** se zobrazí v *databáze SQL*  oddílu.

### <a name="deploy-the-application-to-azure"></a>Nasazení aplikace do Azure

1. Ve Visual Studiu klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** a vyberte **publikovat** v místní nabídce.
  
    ![Publikování v kontextové nabídce projektu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. V **profilu** karty **Publikovat Web** průvodce, klikněte na tlačítko **Microsoft Azure App Service**.
  
    ![Import nastavení publikování](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. Pokud jste nepřidali dříve vašeho předplatného Azure v sadě Visual Studio, proveďte kroky na obrazovce. Tyto kroky aktivují sady Visual Studio pro připojení k vašemu předplatnému Azure tak, která seznam **App Services** bude obsahovat vaše webová stránka.
 
4. Vyberte **předplatné** jste přidali služby App Service, pak **plán služby App Service** složky služby App Service je součástí, a nakonec **služby App Service** sama za nímž následuje **Ok**.

    ![Vyberte službu App Service](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. Po dokončení konfigurace profilu, **připojení** kartě se zobrazí. Klikněte na tlačítko **ověřit připojení** abyste měli jistotu, se používají správná nastavení

    ![Ověření připojení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. Při připojení je potvrzená, se vedle položky zobrazí zelená značka zaškrtnutí **ověřit připojení** tlačítko. Klikněte na tlačítko **Další**.
  
    ![Úspěšně ověřené připojení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. Otevřít **vzdálený připojovací řetězec** rozevíracího seznamu v části **SchoolContext** a vyberte připojovací řetězec pro databázi, který jste vytvořili.
8. Vyberte **aktualizace databáze**.

    ![Karta nastavení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    Toto nastavení způsobí, že proces nasazení pro automatickou konfiguraci aplikace *Web.config* souboru na cílovém serveru tak, aby používala Code First `MigrateDatabaseToLatestVersion` inicializátor třídy.
9. Klikněte na tlačítko **Další**.
10. V **ve verzi Preview** klikněte na tlačítko **spustit Náhled**.
  
    ![Tlačítko StartPreview v kartě náhledu](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    Na kartě se zobrazí seznam souborů, které budou zkopírovány na server. Zobrazení náhledu není vyžadováno pro publikování aplikace, ale je užitečná funkce je potřeba vědět. V takovém případě nemusíte dělat nic se seznam souborů, který se zobrazí. Příště tuto aplikaci nasadíte v tomto seznamu bude pouze soubory, které se změnily.
    ![StartPreview výstupního souboru](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. Klikněte na tlačítko **publikovat**.
    Visual Studio spustí proces kopírování souborů na Azure server.
12. **Výstup** okno zobrazuje, jaké akce nasazení byly provedeny a oznámí úspěšné dokončení nasazení.
  
    ![V okně Výstup hlášením úspěšného nasazení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. Po úspěšném nasazení na adresu URL nasazené webové stránky automaticky otevře výchozí prohlížeč.
    Aplikace, kterou jste vytvořili, je nyní spuštěna v cloudu. 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

V tuto chvíli váš *SchoolContext* databáze byla vytvořena ve službě Azure SQL Database, protože jste vybrali **spustit migrace Code First (spustí se při spuštění aplikace)**. *Web.config* soubor v nasazený web byl změněn tak, aby [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializátor prvním spuštění kódu čtení nebo zápis dat v databázi (která se stalo Pokud jste vybrali **studenty** kartu):

![](https://asp.net/media/4367421/mig.png)

Proces nasazení vytvoří taky nový připojovací řetězec *(SchoolContext\_DatabasePublish*) pro migrace Code First pro aktualizaci schématu databáze a synchronizace replik indexů databáze.

![Database_Publish připojovací řetězec](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Můžete najít nasazenou verzi souboru Web.config na vašem počítači v *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Můžete přistupovat nasazeném *Web.config* vlastní soubor pomocí protokolu FTP. Pokyny najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Postupujte podle pokynů, které začínají znakem "Pokud chcete použít nástroj FTP, potřebujete tři věci: adresa URL protokolu FTP, uživatelské jméno a heslo."

> [!NOTE]
> Webové aplikace neimplementuje zabezpečení, takže každý, kdo najde adresu URL mohou změnit data. Pokyny o tom, jak zabezpečit webovou stránku, naleznete v tématu [nasaďte zabezpečené aplikace ASP.NET MVC pomocí členství, OAuth a SQL Database do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Můžete zabránit ostatním uživatelům z webu pomocí portálu pro správu Azure nebo **Průzkumníka serveru** v sadě Visual Studio Web zastavit.


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>Pokročilé migrace scénáře

Pokud se nasazení databáze spuštěním migrace automaticky, jak je znázorněno v tomto kurzu a provádíte nasazení na web, který běží na několika serverech, získáte víc serverů, které chcete spustit migrace ve stejnou dobu. Migrace jsou atomické, takže pokud dva servery, zkuste spustit stejný migrace, jeden proběhne úspěšně a druhé se nepodaří (za předpokladu, že operace nelze provádět dvakrát). V tomto scénáři Pokud chcete se vyhnout těmto problémům, můžete volat migrace ručně a nastavit váš vlastní kód tak, aby ho situace nastane pouze jednou. Další informace najdete v tématu [skriptování migrace z kódu a spuštění](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) na blogu Rowan Miller a [Migrate.exe](https://msdn.microsoft.com/data/jj618307) (pro provedení migrace z příkazového řádku) na webové stránce MSDN.

Informace o dalších scénářů migrace najdete v tématu [migrace záznam dění na monitoru řady](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="code-first-initializers"></a>První inicializátory kódu

V části nasazení jste viděli, [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) používá inicializátor. Kód nejprve také poskytuje jiné inicializátory, včetně [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (výchozí), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (který jste použili dříve) a [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). `DropCreateAlways` Inicializátor může být užitečné pro nastavení podmínky pro testování částí. Můžete je zapsat také vlastních inicializátory a může volat inicializátor explicitně Pokud nechcete čekat, dokud se čte z aplikace nebo zapisuje do databáze. V době, kdy v tomto kurzu se zápisem do listopadu 2013 můžete použít pouze inicializátory vytvořit a DropCreate dříve než povolíte migrace. Entity Framework tým pracuje na zpřístupnění těchto inicializátory použitelné se také migrace.

Další informace o inicializátory, naleznete v tématu [Principy inicializátory databáze v Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) a kapitola 6 knihu [programování rozhraní Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) podle Julie Lerman a Rowan Miller.

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak povolit migrace a nasazení aplikace. V dalším kurzu zobrazí za přibližně pohledu na pokročilejší témata tak, že rozbalíte datového modelu.

Jak vám v tomto kurzu líbilo a co můžeme zlepšit nám prosím zpětnou vazbu. Můžete také požádat o nový témat na [Show Me jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework lze nalézt v [přístup k datům ASP.NET – doporučené zdroje informací](xref:whitepapers/aspnet-data-access-content-map).

> [!div class="step-by-step"]
> [Předchozí](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [další](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
