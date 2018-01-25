---
uid: whitepapers/aspnet-data-access-content-map
title: "Přístup k datům ASP.NET - doporučené prostředky | Microsoft Docs"
author: rick-anderson
description: "Toto téma obsahuje odkazy na zdroje informací k dokumentaci o tom, jak přistupovat k datům ve webové aplikace ASP.NET, především s použitím rozhraní Entity Framework a SQL Se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 16364951544dff33c9cdb8c8e330cc93de192c47
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-data-access---recommended-resources"></a>Přístup k datům ASP.NET - doporučené prostředky
====================
> Toto téma obsahuje odkazy na zdroje informací k dokumentaci o tom, jak přistupovat k datům ve webové aplikace ASP.NET, především s použitím rozhraní Entity Framework a SQL Server.
> 
> Pokud znáte skvělé blogu, [stackoverflow](http://stackoverflow.com) vlákno nebo jiných odkaz, který by být užitečné, [pošlete nám e-mail](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) s odkazem.
> 
> Aktualizované poslední 4/3/2014


Téma obsahuje následující části:

- [Začínáme s přístup k datům v technologii ASP.NET](#gettingstarted)
- [Pomocí rozhraní Entity Framework](#ef)

    - [Nejdřív pomocí kódu Entity Framework](#cf)
    - [Pomocí migrace Code First Entity Framework](#efcfmigrations)
    - [Nejdřív pomocí databáze Entity Framework nebo Model First (EF designeru)](#efdbf)
    - [Načítání souvisejících dat v rozhraní Entity Framework (opožděného načítání, přes načítání a explicitní načtení)](#efrelateddata)
    - [Optimalizace výkonu Entity Framework](#optimizingef)
    - [Zpracování souběžnost v aplikaci Entity Framework](#efconcurrency)
    - [Publikace o rozhraní Entity Framework](#efbooks)
    - [Další zdroje Entity Framework](#otherefresources)
- [Datové vazby v rozhraní ASP.NET Web Forms aplikace](#wfdatabinding)

    - [Použití rozhraní Web Forms vazby modelu](#wfmodelbinding)
    - [Použití rozhraní Web Forms ovládací prvky zdroje dat](#wfdsc)
    - [Použití rozhraní Web Forms ovládací prvky vázané na Data a výrazy datové vazby](#wfdbc)
- [Práce s databází systému SQL Server](#sqlserver)

    - [Práce s databází systému SQL Server Express LocalDB](#sslocaldb)
    - [Práce s databází systému SQL Server Express](#sse)
    - [Práce s Windows Azure SQL Database](#ssdb)
    - [Volba mezi systému SQL Server a Windows Azure SQL Database](#ssdbchoosing)
- [Práce s systémy správy databáze NoSQL](#nosql)
- [Pomocí dotazů LINQ v aplikacích ASP.NET](#linq)
- [Pomocí uživatelského rozhraní dynamických dat.](#dd)
- [Zabezpečení přístupu k datům](#securing)
- [Optimalizace výkonu Data Access](#optimizingdataaccess)
- [Nasazení databází](#deploying)
- [Přístup k datům prostřednictvím webové služby](#webservice)
- [Další zdroje informací](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Začínáme s přístup k datům v technologii ASP.NET

- [Možnosti ukládání dat (vytváření reálných cloudových aplikací pomocí služby Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Kapitoly zde elektronickou knihu o vývoji pro cloud. Jako alternativu mnoho vývojáři, kteří znají relačních databází mívají přehlédnout zavádí databáze NoSQL. Uvede pokyny na co vezměte v úvahu při výběru relační nebo NoSQL nebo zvolte konkrétní platformu.
- [Možnosti přístupu Data technologie ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Úvod k datům přístup možností pro relační databáze pro technologii ASP.NET a pokyny o tom, jak vyberte platformy a přístup k metody, které jsou vhodné pro váš scénář.
- [Relační databáze](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Pokud jste ještě nepracovali s relačních databází, zobrazí tato stránka Úvod do relační databáze terminologie a koncepty. Úvod do systému SQL Server na konkrétní najdete v části [práce s databází serveru SQL Server](#sqlserver) dál v tomto tématu.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Pomocí rozhraní Entity Framework

- [Entity Framework vývoj přístupy](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Pokyny o tom, jak zvolit Entity Framework vývoj přístup Database First první Model, nebo Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Nejdřív pomocí kódu Entity Framework
  

Ke stažení ukázkové aplikace, nabízejí následující kurzy:

- [Začínáme s EF 6 pomocí MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Obsahuje širokou škálu Entity Framework Code First scénáře, včetně migrace a EF 6 funkce jako je například odolnost připojení, příkaz zachycení a asynchronní. Toto je aktualizovanou verzi [EF 5 / MVC 4 řady](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Starší řady zahrnuje kurz na úložiště a jednotky pracovních vzorů, které není součástí nové řady.
- [Úvod do architektury ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Popisuje užší prvního scénáře rozsah Entity Framework Code, ale nemá komplexnější úlohu zavedení funkce MVC.
- [Vazby modelu a webové formuláře](https://go.microsoft.com/fwlink/?LinkId=286117). V aplikaci Web Forms používá Code First.
- [Začínáme s ASP.NET 4.5 webových formulářů](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Úvod do webových formulářů s některé pokrytí Code First. Používá Model vazby.
- [Úložiště Hudba MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). V aplikaci MVC 3 elektronického obchodu, která také implementuje členství a autorizace používá Code First. Verze MVC a systém členství technologie ASP.NET (ověřování a autorizace) použít se zde jsou zastaralé; Další aktuální informace o členství technologie ASP.NET, najdete v části [https://asp.net/identity](https://asp.net/identity).

Další prostředky:

- [Rozhraní Entity Framework - Code First pro existující databázi](https://msdn.microsoft.com/data/jj200620). MSDN. Video a návod, který ukazuje, jak používat Code First s existující databázi.
- [Středisko pro vývojáře data - rozhraní Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Informace k rozhraní Entity Framework dokumentace, která byla vytvořena a udržovat tým Entity Framework najdete v tématu [Začínáme](https://msdn.microsoft.com/data/ee712907) odkaz.

Viz také [publikace o rozhraní Entity Framework](#efbooks) a [další zdroje Entity Framework](#otherefresources) dál v tomto tématu.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Pomocí migrace Code First Entity Framework
  

Většina Code First kurzy uvedené výše titulní migrace. Další informace najdete v článku na následujících odkazech.

- [Nasazení webu ASP.NET pomocí sady Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). část 2 kurz série, která ukazuje, jak použít migrace Code First pro nasazení databáze.
- [Nasazení aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a databáze SQL na webu systému Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Jak používat migrace nasazení dat členství a aplikací do Azure.
- [Přehled nasazení pro Visual Studio a ASP.NET Web](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Najdete v článku **konfigurace nasazení databáze v sadě Visual Studio** části Další informace o tom, jak je migrace Code First integrovaná do funkce nasazení webové aplikace Visual Studio.
- [Středisko pro vývojáře data - migrace Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Dokumentaci k rozhraní Entity Framework team migrace.
- [Migrace záznam dění na monitoru řady](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF blog). Tři videa na Pokročilá témata v migrace Code First.
- [Migrace Code First s weby webové stránky ASP.NET](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting blog). Ukazuje, jak použít migrace Code First s webem technologie ASP.NET Web Pages umístěním kontextu dat v projektu knihovny tříd sady Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Nejdřív pomocí databáze Entity Framework nebo Model First (EF designeru)

- [Začínáme s Entity Framework 6 Database First pomocí MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Spusťte skript v Průzkumníku serveru k vytvoření databáze a pak použít návrháře Entity Framework pro vytvoření datového modelu. Ukazuje, jak vytvořit jednoduché CRUD webové stránky a pro další data, zpracování funkce, proveďte jeden z kurzů Code First vzhledem k tomu, že všechny pracovní postupy EF použít stejné rozhraní API DbContext.

V následujících zdrojích informací jsou starší. Jsou užitečné, pokud chcete používat verzi rozhraní Entity Framework 4.0 a chcete použít ovládací prvek zdroje dat pro datové vazby v aplikace webových formulářů.

- [Začínáme s rozhraní Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Ukazuje, jak používat **EntityDataSource** ovládacího prvku.
- [Pokračováním rozhraní Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(ukazuje způsob použití **ObjectDataSource** ovládacího prvku. Zahrnuje kurz týkající se zpracování souběžnosti, kurz týkající se výkonu EF a kurz na to, co je nového v EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Zpracování související data v Entity Framework (opožděného načítání, přes načítání a explicitní načtení)

- [Čtení dat souvisejících s platformou Entity Framework v aplikaci ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Kód nejprve ukázkovou aplikaci MVC. Zobrazené metody platí také pro vazby modelu webových formulářů a Database First pracovního postupu.
- [Středisko pro vývojáře data - načítání entit v relaci](https://msdn.microsoft.com/data/jj574232) (MSDN). Dokumentace k rozhraní Entity Framework team o načtení související data.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimalizace výkonu rozhraní Entity Framework

- [Pokročilé scénáře Entity Framework pro aplikaci ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Ukazuje, jak provést vlastní příkazy SQL nebo volání vlastní uložené procedury, jak zakázat detekce změn a zakázání ověření při ukládání změn.
- [Faktory ovlivňující výkon rozhraní Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Faktory ovlivňující výkon (rozhraní Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Tím se maximalizuje výkon s platformou Entity Framework ve webové aplikaci ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Platí pro rozhraní Entity Framework 4.0.
- Viz také [přístup k datům ASP.NET optimalizace](#optimizingdataaccess) dál v tomto tématu.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Zpracování souběžnost v aplikaci Entity Framework

- [Zpracování souběžnosti s platformou Entity Framework v aplikaci ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Nejprve kódu rozhraní API DbContext, v ukázkové aplikaci MVC.
- [Data Developer Center – optimistickou metodu souběžného vzory](https://msdn.microsoft.cus/data/jj592904) (MSDN). Dokumentaci rozhraní Entity Framework team souběžnosti.
- [Zpracování souběžnosti s platformou Entity Framework ve webové aplikaci ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Platí pro rozhraní Entity Framework 4.0. Databáze nejprve ObjectContext API, pomocí ukázkové aplikace webových formulářů.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Publikace o rozhraní Entity Framework

- [Programování Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman a Rowan Lukeš.
- [Programování Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman a Rowan Lukeš.

Obě tyto knih jsou aktuální s aktuální doporučené techniky. Poskytují více komplexní ještě podle Úvod do rozhraní Entity Framework než nic k dispozici na Internetu. Jiné knihy [programovací rozhraní Entity Framework](http://shop.oreilly.com/product/9780596807252.do) Julie Lerman, je větší a komplexnější ale je starší a řadu techniky pokrývá již nejsou doporučeným způsobem, jak používat rozhraní Entity Framework. Viz také seznam doporučení týmu rozhraní Entity Framework na webu knihy [Data Developer Center – knihy](https://msdn.microsoft.com/data/aa937716) na webu MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Další zdroje Entity Framework

- [Blog týmu Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Jeden z nejlepší prostředky pro nejaktuálnější informace a oznámení nová vylepšení. Další související s EF blogy, najdete v tématu Přehled blogů na [začít pracovat s Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [Časopis MSDN](https://msdn.microsoft.com/magazine/default.aspx). Najdete v článku **datových bodů** sloupec, který je často o témata týkající se rozhraní Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Datové vazby v rozhraní ASP.NET Web Forms aplikace

- [ASP.NET – webové formuláře Možnosti přístupu dat](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Použití rozhraní Web Forms vazby modelu

- [Vazby modelu a webové formuláře](https://go.microsoft.com/fwlink/?LinkId=286117). Kurz řada pomocí platformy EF Code First.
- [Webové formuláře Model vazby část 1: Výběr dat](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (Scott Guthrie blog). V tyto starší příspěvky blogu vlastnost, která je aktuálně s názvem ItemType nazýval ModelType, ale jinak informace, které obsahují je platný.
- [Webové formuláře Model vazby část 2: Filtrování dat](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (Scott Guthrie blog).
- [Webové formuláře Model vazby část 3: Aktualizace a ověření](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (Scott Guthrie blog).
- [Vazba modelu 4.5 webových formulářů ASP.NET](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Model vazby část 1 - výběr dat](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Model vazby část 2 - filtrování](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Získávání spuštění pomocí technologie ASP.NET 4.5 webové formuláře – zobrazit Data položky a podrobnosti](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Použití rozhraní Web Forms ovládací prvky zdroje dat

- [Ovládací prvky webového serveru zdroje dat](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Uvedení verze zprostředkovatele Dynamická Data a ovládací prvek EntityDataSource pro Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog vývoj webů společnosti Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Použití rozhraní Web Forms ovládací prvky vázané na Data a výrazy datové vazby

- [Vazby modelu a webové formuláře](https://go.microsoft.com/fwlink/?LinkId=286117). Kurz série, která používá EF Code First.
- [Získávání spuštění pomocí technologie ASP.NET 4.5 webové formuláře – zobrazit Data položky a podrobnosti](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Silně typované ovládací prvky datových](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (Scott Guthrie blog).
- [Silně typované ovládací prvky datových](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 Forms silného typu dat ovládací prvky webového](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [Vázané na data ovládací prvky webového serveru](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Datové vazby výrazy přehled](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Tato stránka obsahuje pouze **Eval** a **vazby**; zatím není aktualizovaná zahrnout **položky** a **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Práce s databází systému SQL Server

- [Funkce databáze serveru SQL](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Pro obecný úvod k široké škále témat systému SQL Server najdete v části položky v rámci této jeden v obsahu.
- [Edice SQL serveru](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Přehled dostupných edicích systému SQL Server s odkazy na další informace o každé z nich.)
- [Připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Pomocí SQL Server Compact pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Databáze produktu ukázky](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Ukázkové databáze AdventureWorks.
- [Instalace ukázkové databáze](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Kromě metod zobrazeny zde můžete také stáhnout jednu z ukázkové soubory .mdf do aplikace\_Data složky webového projektu, převeďte databázi na instanci LocalDB a vytvoření připojovacího řetězce databáze LocalDB. Informace o tom, jak to udělat najdete v tématu [postupy: Upgrade na instanci LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Viz také v následujících částech na práci s SQL Server Express a LocalDB a výběru mezi systému SQL Server a databáze SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Práce s databází systému SQL Server Express LocalDB

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Oficiální Úvod MSDN na instanci LocalDB.
- [Připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Postupy: Upgrade na instanci LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Postup migrace soubor MDF ze starší verze systému SQL Server Express na instanci LocalDB. Máte také projít tento proces, je-li stáhnout jednu z [ukázkové databáze systému SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Představení LocalDB, vylepšené SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog na SQL Server Express). Obsahuje další pozadí na proč byla vytvořena LocalDB, než je součástí webu MSDN.
- [LocalDB: Kde je Moje databáze?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog SQL Server Express). Informace o kterém se vytváří soubory databáze LocalDB.
- [Použití LocalDB s úplnou službu IIS, část 1: uživatelský profil](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog na SQL Server Express). LocalDB nebyla navržena pro práci se službou IIS. Tato série příspěvcích na blogu vysvětluje problémy a některé řešení.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Práce s databází systému SQL Server Express

- [Připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Pokud použijete nastavení AttachDBFileName připojovacího řetězce s SQL Server Express, najdete v části uživatelskou instanci hlavně této stránce.
- [Převzetí vlastnictví vaše místní SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog na SQL Server Express). Běžné problém není schopnost pracovat s databází serveru SQL Server Express, protože nejste správcem v instanci systému SQL Server Express. Ve výchozím nastavení je pouze osoba, která nainstalován systém SQL Server Express správce. Tomto blogu vysvětluje, jak proveďte vlastní správce databáze serveru SQL Server Express, pokud jste správce v počítači.
- [Moje aplikace technologie ASP.NET web databázi můžete použít SQL Server Express v produkčním prostředí?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Práce s Windows Azure SQL Database

- [Nasazení aplikace ASP.NET MVC zabezpečení s členství, OAuth a databáze SQL na webu systému Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (Microsoft Azure site).
- [Databáze SQL](https://docs.microsoft.com/azure/sql-database/) (Microsoft Azure site). Získávání Začínáme kurzy a návody.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Nejvyšší úrovně uzlu obsahu pro databázi SQL na webu MSDN.
- [Windows Azure SQL databáze webu TechNet Wiki články Index](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (webu Microsoft TechNet).
- [Zpracování přechodná chyba aplikace bloku](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Rozhraní, které umožňuje zpracovat přechodných síťových chyb a chyb připojení, pomocí které vzniknou omezení. K dispozici v balíčku NuGet: [Enterprise knihovny 5.0 - přechodné chyby zpracování bloku aplikace](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Začínáme s SQL Database a Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Sada Windows Azure školení](https://www.microsoft.com/download/details.aspx?id=8396) (Microsoft Download Center). Zahrnuje praktická cvičení pro databázi SQL.
- [Windows Azure SQL Database diskuzní fórum](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Přesouvání do systému Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Jeden kapitoly komplexní scénáře začátku do konce týmem Microsoft Patterns and Practices. Zahrnuje proč můžete chtít migrovat a postup migrace ze systému SQL Server k databázi SQL.
- [Migrace databáze systému SQL Server do systému Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Průvodce migrací databáze SQL](http://sqlazuremw.codeplex.com/). Nástroj pro migraci na s otevřeným zdrojem databáze do a z databáze SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Volba mezi systému SQL Server a Windows Azure SQL Database

- [Porovnání systému SQL Server s Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (webu Microsoft TechNet).
- [Migrace dat do služby Windows Azure SQL Database: nástroje a techniky](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Obsahuje oddíly, které chcete porovnat systému SQL Server k databázi SQL a obsahují pokyny, když k migraci ze systému SQL Server do databáze SQL.
- [Příručka k Windows Azure SQL Database doručení](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (webu Microsoft TechNet).
- [Omezení funkcí SQL serveru (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage a systému Windows Azure SQL Database – porovnání a rozdíl od aktualizovaného](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Pro aplikaci, která můžete nasadit do systému Windows Azure může být Windows Azure Table storage alternativu k Windows Azure SQL Database. Toto téma pomůže při rozhodování mezi alternativy.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Pokyny a omezení (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Práce s systémy správy databáze NoSQL

- [Datové služby Windows Azure](https://www.windowsazure.com/develop/net/data/) (Microsoft Azure site). Najdete v článku [Průvodce funkcemi služby Table](https://docs.microsoft.com/azure/) a **velkých objemů dat** části stránky.
- [ASP.NET vícevrstvé aplikace pomocí úložiště tabulky, fronty a objekty BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure site). Kurz začátku do konce s ke stažení ukázkovou aplikaci, která používá tabulky NoSQL úložiště systému Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Pomocí dotazů LINQ v aplikacích ASP.NET

- [Možnosti přístupu Data technologie ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Obsahuje úvod do LINQ.
- [LINQ školení videa](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog Jan Stagner).
- [Fórum ASP.NET vlákno s odkazy na dynamické prostředky LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Pomocí uživatelského rozhraní dynamických dat.

- [Šablony projektu Dynamická Data](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Pokyny k použití projekty Dynamická Data.
- [Dynamická Data technologie ASP.NET](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Zabezpečení přístupu k datům

- [Zabezpečení přístupu k datům v technologii ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Aspekty zabezpečení (rozhraní Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Postupy: Zabezpečené připojovací řetězce při použití ovládacích prvků zdroje dat](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimalizace výkonu Data Access

- [Přehled výkonnostní ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Ukládání do mezipaměti ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Zvýšení výkonu technologie ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Je "Vyřazeno obsah" upozornění v horní části této stránky, ale většina informací je stále relevantní a neexistuje žádné srovnatelné aktualizovaný prostředek.
- [Zlepšení výkonu serveru SQL](https://msdn.microsoft.com/library/ff647793) (MSDN). Komentář stejný jako předchozí odkaz.

Viz také [Entity Framework optimalizace výkonu](#optimizingef) výše v tomto tématu.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Nasazení databází

- [Nasazení webu ASP.NET - doporučené prostředky](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Přístup k datům prostřednictvím webové služby

- [Přístup k datům prostřednictvím webové služby](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Pokyny k použití rozhraní Web API a WCF.
- [Začínáme s rozhraním ASP.NET Web API](../web-api/index.md).
- [Služby WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Další prostředky

- [Přístup k datům ASP.NET – nejčastější dotazy](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET – webové formuláře kurzy - Data](../web-forms/overview/data-access/index.md). Většina tyto kurzy jsou relativně staré; Ujistěte se, abyste si přečetli [možnosti přístupu dat ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) a [možnosti ukládání dat (vytváření reálných cloudových aplikací pomocí služby Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) první tak, aby neobdržíte příliš daleko do metody přístupu data, která není pravé pro váš scénář.
- [Mapa obsahu architektury ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages kurzy - Data](../web-pages/overview/data/index.md).
- [Přístup k datům v sadě Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Obsahuje seznam odkazů podobné Tato mapa obsahu, ale se zaměřením na Visual Studio, nikoli ASP.NET.
