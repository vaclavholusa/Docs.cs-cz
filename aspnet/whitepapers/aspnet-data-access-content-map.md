---
uid: whitepapers/aspnet-data-access-content-map
title: Přístup k datům v technologii ASP.NET – doporučené zdroje informací | Dokumentace Microsoftu
author: rick-anderson
description: Toto téma obsahuje odkazy na dokumentaci o tom, jak přistupovat k datům ve webové aplikace ASP.NET, především s využitím rozhraní Entity Framework a SQL Se...
ms.author: aspnetcontent
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: fb0cea94d82cc8f59ec56a5445ee84d38325995e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832497"
---
<a name="aspnet-data-access---recommended-resources"></a>Přístup k datům v technologii ASP.NET – doporučené zdroje informací
====================
> Toto téma obsahuje odkazy na dokumentaci o tom, jak přistupovat k datům ve webové aplikace ASP.NET, především s využitím rozhraní Entity Framework a systému SQL Server.
> 
> Pokud znáte skvělé blogový příspěvek, [stackoverflow](http://stackoverflow.com) vlákna nebo odkaz, který může být užitečné, [nám pošlete e-mailu](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) s odkazem.
> 
> Aktualizované poslední 4/3/2014


Téma obsahuje následující části:

- [Začínáme s přístupem k datům v technologii ASP.NET](#gettingstarted)
- [Používá nástroj Entity Framework](#ef)

    - [Nejdřív pomocí kód Entity Framework](#cf)
    - [Použití migrace Entity Framework Code First](#efcfmigrations)
    - [Nejdřív pomocí Entity Framework Database nebo Model First (Návrhář EF)](#efdbf)
    - [Načítání souvisejících dat v Entity Framework (opožděné načtení, nemůžou dočkat, až načítání a explicitní načtení)](#efrelateddata)
    - [Optimalizace výkonu Entity Framework](#optimizingef)
    - [Ošetření souběžnosti v aplikaci Entity Framework](#efconcurrency)
    - [Knihy o rozhraní Entity Framework](#efbooks)
    - [Další prostředky Entity Framework](#otherefresources)
- [Vytváření datových vazeb v rozhraní ASP.NET Web Forms aplikací](#wfdatabinding)

    - [Použití webového formuláře vazby modelu](#wfmodelbinding)
    - [Použití webového formuláře ovládací prvky zdroje dat](#wfdsc)
    - [Použití webového formuláře ovládací prvky vázané na Data a výrazy vázání dat](#wfdbc)
- [Práce s databází SQL serveru](#sqlserver)

    - [Práce s databází serveru SQL Server Express LocalDB](#sslocaldb)
    - [Práce s databází systému SQL Server Express](#sse)
    - [Pracovat se službou Windows Azure SQL Database](#ssdb)
    - [Volba mezi SQL serverem a Windows Azure SQL Database](#ssdbchoosing)
- [Práce se systémy pro správu databází NoSQL](#nosql)
- [Pomocí dotazů LINQ v aplikacích ASP.NET](#linq)
- [Používat Dynamická Data generování uživatelského rozhraní](#dd)
- [Zabezpečení přístupu k datům](#securing)
- [Optimalizace výkonu Data Access](#optimizingdataaccess)
- [Nasazení databáze](#deploying)
- [Přístup k datům prostřednictvím webové služby](#webservice)
- [Další zdroje informací](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Začínáme s přístupem k datům v technologii ASP.NET

- [Možnosti úložiště dat (vytváření skutečných cloudových aplikací pomocí Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Kapitola elektronickou knihu o vývoji pro cloud. Zavádí databáze NoSQL jako alternativu mnoho vývojáři, kteří znají relačních databází obvykle přehlédnout. Uvede počet pokyny na co zamyslet, při výběru relační nebo NoSQL nebo zvolte konkrétní platformu.
- [Možnosti přístupu k datům v technologii ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Úvod do možnosti přístupu k datům pro relační databáze pro ASP.NET a pokyny o tom, jak zvolit platformami a přístup k metodám, které jsou vhodné pro váš scénář.
- [Relační databáze](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Pokud jste ještě nepracovali s relační databáze, najdete na této stránce s úvodem do relační databáze terminologie a koncepty. Úvod do systému SQL Server zejména naleznete v tématu [práce s databázemi serveru SQL Server](#sqlserver) dále v tomto tématu.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Používá nástroj Entity Framework

- [Vývojové přístupy s Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Pokyny, jak zvolit Entity Framework vývoj metodiky Database First, Model první nebo Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Nejdřív pomocí kód Entity Framework
  

V následujících kurzech nabídky ukázkové aplikace ke stažení:

- [Začínáme s EF 6 pomocí MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Zahrnuje širokou škálu scénářů Entity Framework Code First, včetně migrace a EF 6 funkce jako je asynchronní, odolnost připojení a zachycení příkazů. Toto je aktualizovanou verzi [EF 5 / MVC 4 řady](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Starší kurz obsahuje na úložiště a jednotek práce vzory, které není součástí nové řady.
- [Úvod do architektury ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Řeší užší prvního scénáře rozsah Entity Framework Code, ale nemá komplexnější úlohu zavedení funkcemi technologie MVC.
- [Vazby modelu a webové formuláře](https://go.microsoft.com/fwlink/?LinkId=286117). Používá Code First v aplikaci webových formulářů.
- [Začínáme s webovými formuláři ASP.NET 4.5](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Úvod do webových formulářů s některé pokrytí Code First. Použití vazby modelu.
- [Aplikace MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Používá Code First v aplikaci elektronického obchodování MVC 3, která také implementuje členství a ověřování. Verze MVC a systém členství technologie ASP.NET (ověřování a autorizace) se tady použít jsou zastaralá. aktuální informace o členství technologie ASP.NET naleznete v tématu [ https://asp.net/identity ](https://asp.net/identity).

Další materiály:

- [Entity Framework - Code First pro existující databázi](https://msdn.microsoft.com/data/jj200620). MSDN. Videa a návod, který ukazuje, jak používat Code First s existující databázi.
- [Středisko pro vývojáře dat – Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Pokyny k dokumentaci rozhraní Entity Framework, která byla vytvořena a udržované týmem Entity Framework, najdete v článku [Začínáme](https://msdn.microsoft.com/data/ee712907) odkaz.

Viz také [knihy o rozhraní Entity Framework](#efbooks) a [další prostředky Entity Framework](#otherefresources) dále v tomto tématu.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Použití migrace Entity Framework Code First
  

Většina z uvedených titulní migrace Code First kurzů. Další informace najdete v článku na následujících odkazech.

- [Nasazení webu ASP.NET pomocí sady Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 2 – část série kurzů, který ukazuje, jak nasadit databázi pomocí migrace Code First.
- [Nasaďte zabezpečené aplikace ASP.NET MVC 5 s Membership, OAuth a SQL Database na web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Jak používat migrace nasazení dat. členství a aplikace do Azure.
- [Přehled nasazení pro Visual Studio a ASP.NET Web](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Najdete v článku **konfigurace nasazení databáze v sadě Visual Studio** oddíl pro vysvětlení, jak je migrace Code First integrovaná do funkce nasazení webové aplikace Visual Studio.
- [Středisko pro vývojáře dat – migrace Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Dokumentace ke službě migrace Entity Framework týmu.
- [Záznam dění na monitoru řady migrace](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF blog). Tři videa na Pokročilá témata týkající se migrace Code First.
- [Kód migrace First s ASP.NET Web Pages weby](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting blog). Ukazuje, jak použít migrace Code First s webem rozhraní ASP.NET Web Pages vložením kontext dat v projektu knihovny tříd sady Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Nejdřív pomocí Entity Framework Database nebo Model First (Návrhář EF)

- [Začínáme s Entity Framework 6 Database First pomocí MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Spuštění skriptu v Průzkumníku serveru k vytvoření databáze a pak použít k vytvoření datového modelu Entity Framework designer. Ukazuje, jak vytvořit jednoduché CRUD webové stránky a pro další data, zpracování funkce, můžete postupovat podle některého Code First kurzy od všech pracovních postupů EF pomocí stejného DbContext rozhraní API.

V následujících zdrojích informací jsou starší. Jsou užitečné, pokud chcete používat verzi rozhraní Entity Framework 4.0 a chcete použít ovládací prvek zdroje dat pro datové vazby v aplikaci webových formulářů.

- [Začínáme s Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Ukazuje způsob použití **EntityDataSource** ovládacího prvku.
- [Pokračujte v Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(ukazuje způsob použití **ObjectDataSource** ovládacího prvku. Obsahuje kurz týkající se souběžného zpracování, kurz týkající se výkonu EF a kurz na novinky v EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Zpracování souvisejících dat v Entity Framework (opožděné načtení, nemůžou dočkat, až načítání a explicitní načtení)

- [Čtení souvisejících dat s Entity Framework v aplikaci ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Kód nejprve ukázkovou aplikaci MVC. Metody uvedené platí také pro vazby modelu webových formulářů a databáze prvního pracovního postupu.
- [Středisko pro vývojáře dat – Načtení souvisejících entit](https://msdn.microsoft.com/data/jj574232) (MSDN). Dokumentaci rozhraní Entity Framework týmu o načítání související data.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimalizace výkonu Entity Framework

- [Pokročilé scénáře pro Entity Framework pro aplikaci ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Ukazuje, jak spustit vlastní příkazy SQL nebo zavolat vlastní uložené procedury, jak zakázat detekce změn a jak zakázat ověřování při ukládání změn.
- [Faktory ovlivňující výkon u Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Důležité informace o výkonu (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Dosažení maximálního výkonu se sadou Entity Framework ve webové aplikaci ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Platí pro Entity Framework 4.0.
- Viz také [přístup k datům ASP.NET optimalizace](#optimizingdataaccess) dále v tomto tématu.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Ošetření souběžnosti v aplikaci Entity Framework

- [Ošetření souběžnosti se sadou Entity Framework v aplikaci ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Kód nejprve DbContext rozhraní API, pomocí ukázkové aplikace MVC.
- [Středisko pro vývojáře dat – optimistického řízení souběžnosti vzorů](https://msdn.microsoft.cus/data/jj592904) (MSDN). Dokumentace ke službě team Entity Framework souběžnosti.
- [Ošetření souběžnosti se sadou Entity Framework ve webové aplikaci ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Platí pro Entity Framework 4.0. Databáze nejprve ObjectContext rozhraní API, pomocí ukázkové aplikace webových formulářů.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Knihy o rozhraní Entity Framework

- [Entity Framework programování: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman a Rowan Miller.
- [Entity Framework programování: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman a Rowan Miller.

Obě tyto knih jsou aktuální s aktuální doporučené postupy. Poskytují více komplexní ještě postupujte Úvod do rozhraní Entity Framework než nic k dispozici na Internetu. Další adresáře [programování rozhraní Entity Framework](http://shop.oreilly.com/product/9780596807252.do) Julie Lerman, je větší a komplexnější ale to je starší a řadu technik zahrnuje už nejsou doporučeným způsobem, jak použít rozhraní Entity Framework. Podívejte se také seznam knihy doporučuje týmem Entity Frameworku na [středisko pro vývojáře dat – knihy](https://msdn.microsoft.com/data/aa937716) na webu MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Další prostředky Entity Framework

- [Blog týmu Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Jedna z nejlepších zdrojů pro nejaktuálnější informace a oznámení o nových vylepšeních. Další související s EF blogy, naleznete v tématu Přehled blogů na [Začínáme s Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [Zpravodaj MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Zobrazit **datových bodů** sloupec, který je často témata související s Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Vytváření datových vazeb v rozhraní ASP.NET Web Forms aplikací

- [Webové formuláře ASP.NET možnosti přístupu k datům](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Použití webového formuláře vazby modelu

- [Vazby modelu a webové formuláře](https://go.microsoft.com/fwlink/?LinkId=286117). Pomocí platformy EF Code First série kurzů.
- [Webové formuláře modelu vazby část 1: Výběr dat](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blogu Scotta Guthrie). V tyto starší blogové příspěvky vlastnost, která je aktuálně s názvem ItemType označovala jako ModelType, ale jinak je informace, které obsahují platné.
- [Webové formuláře modelu vazby část 2: Filtrování dat](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blogu Scotta Guthrie).
- [Webové formuláře modelu vazby část 3: Aktualizace a ověření](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blogu Scotta Guthrie).
- [Vazby modelu webové formuláře ASP.NET 4.5](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Vazby modelu. část 1 – výběr dat](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Vazby modelu. část 2 – filtrování](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Začínáme pracovat s webovými formuláři ASP.NET 4.5 – zobrazení dat položek a podrobnosti](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Použití webového formuláře ovládací prvky zdroje dat

- [Ovládací prvky webového serveru zdroje dat](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Oznamujeme vydání zprostředkovatele dynamických dat a ovládací prvek třídu EntityDataSource platí pro Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (vývoj pro Web Microsoft blogu).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Použití webového formuláře ovládací prvky vázané na Data a výrazy vázání dat

- [Vazby modelu a webové formuláře](https://go.microsoft.com/fwlink/?LinkId=286117). Série kurzů, které používá platforem EF Code First.
- [Začínáme pracovat s webovými formuláři ASP.NET 4.5 – zobrazení dat položek a podrobnosti](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Silně typované ovládací prvky dat](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blogu Scotta Guthrie).
- [Silně typované ovládací prvky dat](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 webové formuláře silného typu ovládací prvky dat](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [Vázané na data ovládací prvky webového serveru](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Datové vazby Přehled výrazů](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Tato stránka se vztahuje pouze **Eval** a **svázat**; ještě není aktualizovaný zahrnout **položky** a **položku BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Práce s databází SQL serveru

- [Funkce služby SQL Server Database](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Obecný úvod k celou řadu témat SQL serveru naleznete v tématu položky v rámci tohoto objektu v obsahu.
- [Edice SQL serveru](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Souhrn k dispozici edicích systému SQL Server s odkazy na další informace o každé z nich.)
- [Připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Pro webové aplikace ASP.NET pomocí SQL Server Compact](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Databáze ukázky produktu](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Ukázkové databáze AdventureWorks.
- [Instalace ukázkových databází](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Kromě metod, které je znázorněno zde, můžete si také stáhnout jednu z ukázkových souborů .mdf do aplikace\_Data složce webového projektu, převeďte databázi na instanci LocalDB a vytvoření připojovacího řetězce LocalDB. Informace o tom, jak to udělat, najdete v části [postupy: Upgrade na instanci LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Viz také v dalších částech na práci s SQL Server Express LocalDB a výběru mezi SQL Server a SQL Database.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Práce s databází serveru SQL Server Express LocalDB

- [Systém SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Oficiální Úvod MSDN na instanci LocalDB.
- [Připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Postupy: Upgrade na instanci LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Jak migrovat souboru .mdf ze starší verze systému SQL Server Express LocalDB. Budete také muset projít tento proces, pokud si stáhněte některou z [ukázkové databáze systému SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Představení LocalDB, vylepšení SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog SQL Server Express). Obsahuje další informace o proč byla vytvořena LocalDB, než kolik obsahuje na webu MSDN.
- [LocalDB: Kde je Moje databáze?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog SQL Server Express). Informace o kde jsou vytvořeny soubory databáze LocalDB.
- [Použití LocalDB s úplnou službu IIS, část 1: profil uživatele](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog SQL Server Express). LocalDB není určená pro práci se službou IIS. Tuto sérii blogových příspěvků vysvětluje tyto problémy a některé alternativní řešení.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Práce s databází systému SQL Server Express

- [Připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Pokud používáte nastavení AttachDBFileName připojovacího řetězce s SQL Server Express, v části zejména uživatelské Instance této stránky.
- [Jak převzít vlastnictví vaší místní SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog SQL Server Express). Běžný problém se nebudou moct pracovat s databází serveru SQL Server Express, protože nejste správcem na instanci systému SQL Server Express. Ve výchozím nastavení pouze osoba, která nainstalován systém SQL Server Express je správce. Tento blog vysvětluje, jak provést sami na správce systému SQL Server Express, pokud jste správce v počítači.
- [Moje webová aplikace ASP.NET databázi můžete použít SQL Server Express v produkčním prostředí?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Pracovat se službou Windows Azure SQL Database

- [Nasaďte aplikaci zabezpečení rozhraní ASP.NET MVC s nástroji Membership, OAuth a SQL Database na web Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (webu Microsoft Azure).
- [Databáze SQL](https://docs.microsoft.com/azure/sql-database/) (webu Microsoft Azure). Získávání začít kurzy a příručky s postupy.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Uzel nejvyšší úrovně obsahu pro službu SQL Database na webu MSDN.
- [Windows Azure SQL Database TechNet Wiki články Index](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (webu Microsoft TechNet).
- [Blok aplikací zpracování přechodných chyb](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Systém, který vám umožní vám zpracovávat přechodných síťových chyb a chyb připojení z omezování výsledku. K dispozici v balíčku NuGet: [Enterprise Library 5.0 - přechodné Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Začínáme se službou SQL Database a Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure školicí sada](https://www.microsoft.com/download/details.aspx?id=8396) (Microsoft Download Center). Obsahuje praktická cvičení pro SQL Database.
- [Windows Azure SQL Database komunitním fóru](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Přechod na Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Jedné kapitoly komplexní scénáře začátku do konce týmem Microsoft Patterns and Practices. Popisuje, proč může být vhodné k migraci a postupu při migraci z SQL serveru do služby SQL Database.
- [Migrace databáze systému SQL Server do služby Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Průvodce migrací služby SQL Database](http://sqlazuremw.codeplex.com/). Open source nástroj pro migraci databází do a z databáze SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Volba mezi SQL serverem a Windows Azure SQL Database

- [Porovnání SQL serveru s Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (webu Microsoft TechNet).
- [Migrace dat do služby Windows Azure SQL Database: nástroje a techniky](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Obsahuje oddíly, které porovnání SQL serveru do služby SQL Database a poskytuje pokyny, kdy k migraci z SQL serveru do služby SQL Database.
- [Příručka k Windows Azure SQL Database doručování](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (webu Microsoft TechNet).
- [Omezení funkcí SQL serveru (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage a Windows Azure SQL Database – porovnání a rozdíly](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Pro aplikace, který nasadíte do Azure s Windows Windows Azure Table storage může být alternativu ke službě Windows Azure SQL Database. Toto téma vám pomůže rozhodnout mezi tyto alternativy.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Pokyny a omezení (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Práce se systémy pro správu databází NoSQL

- [Data Services – Azure Windows](https://www.windowsazure.com/develop/net/data/) (webu Microsoft Azure). Zobrazit [Průvodce funkcemi služby Table Service](https://docs.microsoft.com/azure/) a **velké objemy dat** části stránky.
- [ASP.NET vícevrstvé aplikace pomocí úložiště tabulky, fronty a objekty BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (webu Microsoft Azure). Kurz k začátku do konce pomocí ke stažení ukázkové aplikace, která používá tabulky NoSQL úložiště Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Pomocí dotazů LINQ v aplikacích ASP.NET

- [Možnosti přístupu k datům v technologii ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Obsahuje úvod do LINQ.
- [Školicí videa LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog Joe Stagner).
- [Vlákna ve fóru ASP.NET s odkazy na dynamické prostředky LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Používat Dynamická Data generování uživatelského rozhraní

- [Šablony projektů dynamických dat](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Pokyny, kdy se má použít dynamické databázové projekty.
- [Dynamická Data technologie ASP.NET](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Zabezpečení přístupu k datům

- [Zabezpečení přístupu k datům v technologii ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Důležité informace o zabezpečení (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Postupy: Zabezpečené připojovací řetězce, jestli používáte ovládací prvky zdroje dat](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimalizace výkonu Data Access

- [ASP.NET: přehled výkonu](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Ukládání do mezipaměti ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Zlepšení výkonu technologie ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Existuje upozornění "Vyřazený obsah" v horní části této stránky, ale většina informací je stále relevantní a neexistuje žádné srovnatelné aktualizovaný prostředek.
- [Zlepšení výkonu SQL serveru](https://msdn.microsoft.com/library/ff647793) (MSDN). Komentář stejný jako předchozí odkaz.

Viz také [Entity Framework optimalizace výkonu](#optimizingef) výše v tomto tématu.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Nasazení databáze

- [Nasazení webu ASP.NET – doporučené zdroje informací](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Přístup k datům prostřednictvím webové služby

- [Přístup k datům prostřednictvím webové služby](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Pokyny k použití webového rozhraní API a WCF.
- [Začínáme s rozhraním ASP.NET Web API](../web-api/index.md).
- [Služby WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Další prostředky

- [Přístup k datům ASP.NET – nejčastější dotazy](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Webové formuláře ASP.NET výukových kurzů – Data](../web-forms/overview/data-access/index.md). Většina těchto kurzů je poměrně staré; Nezapomeňte si přečíst [možnosti přístupu k datům ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) a [možnosti úložiště dat (vytváření skutečných cloudových aplikací pomocí Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) první tak, aby vám podrobněji metodu přístupu data, která není správné pro váš scénář.
- [Mapa obsahu služby technologie ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET – webové stránky výukových kurzů – Data](../web-pages/overview/data/index.md).
- [Přístup k datům v sadě Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Obsahuje seznam odkazů podobně jako tato mapa obsahu, ale se zaměřením na Visual Studio a nikoli technologie ASP.NET.
