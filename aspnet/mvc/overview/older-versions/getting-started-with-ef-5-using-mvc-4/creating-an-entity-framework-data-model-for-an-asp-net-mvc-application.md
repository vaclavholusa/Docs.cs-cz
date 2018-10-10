---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Vytvoření datového modelu Entity Framework pro aplikace ASP.NET MVC (1 10) | Dokumentace Microsoftu
author: tdykstra
description: Novější verzi v této sérii kurzů je k dispozici pro sadu Visual Studio 2013, Entity Framework 6 a MVC 5. Contoso University ukázkové webové aplikaci de...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b691f718258f98e03513a089ca26b286f284765e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913230"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Vytvoření datového modelu Entity Framework pro aplikace ASP.NET MVC (1 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A [novější verzi v této sérii kurzů](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) je k dispozici pro sadu Visual Studio 2013, Entity Framework 6 a MVC 5.
> 
> 
> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 a Visual Studio 2012. Ukázková aplikace je webovou stránku pro fiktivní společnosti Contoso University. Zahrnuje funkce, jako student přijetí, kurz vytvoření a přiřazení instruktorem. Tato série kurzů vysvětluje, jak vytvořit ukázková aplikace Contoso University. Je možné [stáhnout hotovou aplikaci](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Kód nejprve
> 
> Existují tři způsoby, jak můžete pracovat s daty v Entity Framework: *Database First*, *první Model*, a *Code First*. Tento kurz je určen pro Code First. Informace o rozdílech mezi těmito pracovními postupy a pokyny o tom, jak vybrat tu nejlepší pro váš scénář, naleznete v tématu [Entity Framework vývojářské pracovní postupy](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Vzorová aplikace je sestavena [ASP.NET MVC](../../../index.md). Pokud budete chtít pracovat s modelu webových formulářů ASP.NET, přečtěte si článek [vazby modelu a webové formuláře](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) série kurzů a [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> | **Uvedené v tomto kurzu** | **Funguje taky s** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express pro Web. Toto je automaticky nainstalován ve Windows Azure SDK Pokud ještě nemáte sadu VS 2012 nebo VS 2012 Express pro Web. Visual Studio 2013 by měla fungovat, ale kurzu nebyl byly testovány s ním a některé možnosti nabídky a dialogových oknech se liší. [VS 2013 verze sady Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) je vyžadován pro nasazení Windows Azure. |
> | .NET 4.5 | Většina funkcí je znázorněno bude fungovat v rozhraní .NET 4, ale některé nebude. Například výčet podporovaných v EF vyžaduje .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Pokud přeskočíte kroky nasazení Windows Azure, není nutné sadu SDK. Po vydání nové verze sady SDK, na odkaz se nainstaluje na novější verzi. V takovém případě budete muset přizpůsobit některé z pokynů na nové uživatelské rozhraní a funkcí. |
> 
> ## <a name="questions"></a>Dotazy
> 
> Pokud máte nějaké otázky, které přímo nesouvisejí, najdete v tomto kurzu, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework a LINQ to Entities fórum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), nebo [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Potvrzení
> 
> Zobrazit poslední kurz v této sérii pro [potvrzení a poznámky o VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Původní verze tohoto kurzu
> 
> Je k dispozici v původní verze tohoto kurzu [EF 4.1 / MVC 3 (elektronická kniha)](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Webová aplikace Contoso University

Aplikace, kterou je budete vytvářet v těchto kurzech je webová stránka jednoduché university.

Uživatelé mohou zobrazit a aktualizovat Všichni studenti, kurz a informace instruktorem. Tady je několik obrazovek, které vytvoříte.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Styl uživatelského rozhraní tohoto webu má blízko co je generována pomocí integrovaných šablon, ukládají, abyste se mohli zaměřit kurz hlavně na tom, jak používat rozhraní Entity Framework.

## <a name="prerequisites"></a>Požadavky

Pokyny a snímky obrazovky v tomto kurzu se předpokládá, že používáte [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) nebo [Visual Studio 2012 Express pro Web](https://go.microsoft.com/fwlink/?LinkID=275131)s nejnovější aktualizací a sady Azure SDK pro .NET nainstalovat od července 2013. Můžete získat všechny informace o pomocí následujícího odkazu:

[Azure SDK pro platformu .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Pokud máte nainstalovanou sadu Visual Studio, na odkaz výše nainstaluje všechny chybějící komponenty. Pokud nemáte Visual Studio, na odkaz nainstaluje Visual Studio 2012 Express pro Web. Můžete použít Visual Studio 2013, ale některé požadované procedury a obrazovky se budou lišit.

## <a name="create-an-mvc-web-application"></a>Vytvořit webovou aplikaci MVC

Otevřete Visual Studio a vytvořte nový projekt C# s názvem "ContosoUniversity" pomocí **webové aplikace ASP.NET MVC 4** šablony. Ujistěte se, že je cílem **rozhraní .NET Framework 4.5** (budete používat [ `enum` vlastnosti](https://msdn.microsoft.com/data/hh859576.aspx), důvěřuje a která vyžaduje rozhraní .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

V **nového projektu ASP.NET MVC 4** dialogové okno Vyberte **internetovou aplikaci** šablony.

Nechte **Razor** zobrazit vybraný modul a dál necháte **vytvořit projekt testování částí** políčko zaškrtnuto.

Klikněte na tlačítko **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Nastavit styl lokality

Několik jednoduchých změn se nastavit v nabídce webu, rozložení a domovské stránky.

Otevřít *Views\Shared\\_Layout.cshtml*a nahraďte obsah souboru následující kód. Změny jsou zvýrazněné.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Tento kód provede následující změny:

- Nahradí instance šablony "Moje aplikace ASP.NET MVC" a "zde bude vaše logo" "University společnosti Contoso".
- Přidá několik odkazy na akce, které se použije v pozdější části kurzu.

V *Views\Home\Index.cshtml*, nahraďte obsah souboru následující kód k odstranění šablony odstavců o ASP.NET a MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

V *Controllers\HomeController.cs*, změňte hodnotu `ViewBag.Message` v `Index` metody akce k "Vítejte Contoso University!", jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Stisknutím klávesy CTRL + F5 pro spuštění tohoto webu. Zobrazí domovská stránka s hlavní nabídky.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Vytvoření datového modelu

Dále vytvoříte tříd entit pro aplikaci Contoso University. Začnete s následující tři entity:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Existuje vztah jeden mnoho mezi `Student` a `Enrollment` entity, a existuje vztah jeden mnoho mezi `Course` a `Enrollment` entity. Jinými slovy student možné zaregistrovat libovolný počet kurzy a kurzu může mít libovolný počet studentů zaregistrovaná do něj.

V následujících částech vytvoříte třídu pro každou z těchto entit.

> [!NOTE]
> Pokud se pokusíte ke kompilaci projektu před dokončením vytvoření všech těchto tříd entit, získáte chyby kompilátoru.


### <a name="the-student-entity"></a>Entita studenta

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

V *modely* složku, vytvořte *Student.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` Vlastnost se stane sloupec primárního klíče tabulky databáze, která odpovídá této třídy. Ve výchozím nastavení interpretuje Entity Framework vlastnost s názvem `ID` nebo *classname* `ID` jako primární klíč.

`Enrollments` Je vlastnost *navigační vlastnost*. Vlastnosti navigace podržte dalšími subjekty, které se vztahují k této entity. V takovém případě `Enrollments` vlastnost `Student` entita bude obsahovat všechny `Enrollment` entity, které se vztahují k, které `Student` entity. Jinými slovy Pokud daný `Student` řádků v databázi má dva související `Enrollment` řádky (hodnota řádky, které obsahují student získal primární klíč v jejich `StudentID` sloupec cizího klíče), který `Student` entity `Enrollments` navigační vlastnost bude obsahovat tyto dvě `Enrollment` entity.

Navigační vlastnosti se obvykle definují jako `virtual` tak, aby se můžete využít některé funkce Entity Framework, jako *opožděné načtení*. (Opožděné načtení budou vysvětlena dále v [čtení souvisejících dat](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) později v této sérii kurzů.

Pokud vlastnost navigace může obsahovat více entit (jako v relace m: n nebo 1 n), jeho typ musí být seznam, ve kterém položky lze přidávat, odstranit a aktualizovat, například `ICollection`.

### <a name="the-enrollment-entity"></a>Registrace Entity

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

V *modely* složku, vytvořte *Enrollment.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Vlastnost na podnikové úrovni je [výčtu](https://msdn.microsoft.com/data/hh859576.aspx). Otazník po `Grade` deklarace typu znamená, že `Grade` vlastnost [s možnou hodnotou Null](https://msdn.microsoft.com/library/2cf62fcy.aspx). Se liší od nulovou na podnikové úrovni na podnikové úrovni, který má hodnotu null – hodnota null znamená známku vyjádřenou není znám nebo ještě nebyly přiřazeny.

`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost je `Student`. `Enrollment` Entita je přidružený nejméně k jednomu `Student` entity, tak vlastnost může obsahovat pouze jeden `Student` entity (na rozdíl od `Student.Enrollments` navigační vlastnost předchozímu příkladu, který může obsahovat více `Enrollment` entity).

`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost je `Course`. `Enrollment` Entita je přidružený nejméně k jednomu `Course` entity.

### <a name="the-course-entity"></a>Kurz Entity

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

V *modely* složku, vytvořte *Course.cs*, nahraďte existující kód následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` Je navigační vlastnost. A `Course` entit může souviset s libovolným počtem `Enrollment` entity.

Budete říkáme, informace o [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Žádný)] atribut v dalším kurzu. V podstatě tento atribut umožňuje zadat primární klíč pro kurz namísto nutnosti databáze jeho vygenerování.

## <a name="create-the-database-context"></a>Vytvořte kontext databáze

Hlavní třída, která koordinuje funkce Entity Framework pro daný datový model je *kontext databáze* třídy. Vytvoření této třídy odvozené z [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) třídy. V kódu určíte entit, které jsou zahrnuty v datovém modelu. Můžete také přizpůsobit chování určité Entity Framework. V tomto projektu je s názvem třídy `SchoolContext`.

Vytvořte složku s názvem *DAL* (pro Data Access Layer). V této složce vytvořte nový soubor třídy *SchoolContext.cs*a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Tento kód vytvoří [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) vlastností pro každou sadu entit. V terminologii Entity Framework *sadu entit* obvykle odpovídá tabulku databáze a *entity* odpovídající řádek v tabulce.

`modelBuilder.Conventions.Remove` Výroky [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda zabraňuje se pluralized názvy tabulek. Pokud jste to neudělali, by se pojmenoval generované tabulky `Students`, `Courses`, a `Enrollments`. Místo toho budou názvy tabulek `Student`, `Course`, a `Enrollment`. Vývojáři Nesouhlasím o tom, jestli by měl názvy tabulek pluralized nebo ne. Tento kurz používá jednotný tvar, ale důležité je, že můžete vybrat libovolný formulář dáváte přednost zahrnutím nebo vynechání tento řádek kódu.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) je Odlehčená verze SQL serveru Express databázového stroje, který začíná na vyžádání a běží v uživatelském režimu. LocalDB běží v ve speciálním režimu provádění SQL Server Express, která umožňuje pracovat s databází jako *.mdf* soubory. Obvykle jsou uloženy soubory databáze LocalDB *aplikace\_Data* složce webového projektu. Funkce instance uživatele v SQL serveru Express také umožňuje pracovat s *.mdf* soubory, ale uživatelské instance funkce je zastaralá možnost; proto se doporučuje LocalDB pro práci s *.mdf* soubory.

Obvykle SQL Server Express se nepoužívá pro produkční webové aplikace. LocalDB zejména se nedoporučuje pro produkční použití s webovou aplikací vzhledem k tomu, že není určená pro práci se službou IIS.

V sadě Visual Studio 2012 a novějších verzích je LocalDB nainstalované ve výchozím nastavení se sadou Visual Studio. V sadě Visual Studio 2010 a starší SQL Server Express (bez LocalDB) je nainstalovaná ve výchozím nastavení se sadou Visual Studio; je nutné ji nainstalovat ručně, pokud používáte Visual Studio 2010.

V tomto kurzu budete pracovat s LocalDB tak, aby databáze může být uložená v *aplikace\_Data* složky jako *.mdf* souboru. Otevřít kořenové *Web.config* a přidejte nový připojovací řetězec k `connectionStrings` kolekce, jak je znázorněno v následujícím příkladu. (Nezapomeňte aktualizovat *Web.config* soubor v kořenové složce projektu. K dispozici je také *Web.config* soubor *zobrazení* podsložky, která není nutné aktualizovat.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Ve výchozím nastavení, Entity Framework hledá připojovací řetězec se stejným názvem jako `DbContext` třídy (`SchoolContext` pro tento projekt). Připojovací řetězec, který jste přidali Určuje databázi LocalDB s názvem *ContosoUniversity.mdf* umístěné v *aplikace\_Data* složky. Další informace najdete v tématu [připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Není nutné skutečně zadejte připojovací řetězec. Pokud nezadáte připojovací řetězec, Entity Framework ho vytvoří za vás. však nemusí být databáze ve *aplikace\_data* složky vaší aplikace. Informace o s novou databází najdete v tématu [Code First pro novou databázi](https://msdn.microsoft.com/data/jj193542).

`connectionStrings` Kolekci má také připojovací řetězec s názvem `DefaultConnection` používaný pro databázi členství. Nebudete používat databázi členství v tomto kurzu. Jediným rozdílem mezi dva připojovací řetězce je název databáze a hodnota atributu name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Nastavit a spustit migraci Code First

Při prvním spuštění pro vývoj aplikací, datový model změn často a pokaždé, když změny modelu, který získá synchronizován s databází. Můžete nakonfigurovat automaticky vyřadit a znovu vytvořit databázi pokaždé, když změníte datový model Entity Framework. To není problém v rané fázi vývoje, protože je snadno znovu vytvořit testovací data, ale po nasazení do produkčního prostředí obvykle chcete aktualizovat schéma databáze bez vyřazení databáze. Povolí funkci migrace Code First pro aktualizaci databáze bez vyřadit a vytvoříte ho znovu. V rané fázi vývojového cyklu nového projektu můžete chtít použít [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) vyřadit, znovu vytvořit a naplnit databázi znovu pokaždé, když změny modelu. Jeden získáte připravená k nasazení vaší aplikace, můžete převést na postup migrace. Pro účely tohoto kurzu budete používat pouze migrace. Další informace najdete v tématu [migrace Code First](https://msdn.microsoft.com/data/jj591621) a [migrace záznam dění na monitoru řady](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Povolení migrace Code First

1. Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet** a potom **Konzola správce balíčků**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Na `PM>` řádku zadejte následující příkaz:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![příkaz povolení migrace](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Tento příkaz vytvoří *migrace* složky v projektu ContosoUniversity a umístí v této složce *Configuration.cs* souboru, který můžete upravit konfiguraci migrace.

    ![Migrace složky](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration` Obsahuje třídy `Seed` metodu, která je volána při vytvoření databáze a pokaždé, když se aktualizuje po datového modelu změnit.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Účelem tohoto `Seed` metodou je vám umožní vložit testovací data do databáze po Code First ji vytvoří nebo aktualizuje.

### <a name="set-up-the-seed-method"></a>Nastavit Seed – metoda

[Počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda spustí, když migrace Code First vytvoří databázi a pokaždé, když se aktualizuje databáze na nejnovější migrace. Účelem Seed – metoda je vám k vložení dat do tabulky před aplikace má přístup k databázi poprvé.

V dřívějších verzích Code First, před migrací, bylo běžné `Seed` metody k vložení dat testu, protože při každé změně modelu během vývoje databáze byl zcela odstranit a znovu vytvářet úplně od začátku. Pomocí migrace Code First, testovací data se uchovávají po provedení změn databáze, takže včetně testovací data v [počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodu není obvykle nutné. Ve skutečnosti nechcete, aby `Seed` metoda k vložení dat testu, pokud budete používat migrace nasazení databáze do produkčního prostředí, protože `Seed` metoda bude spuštěna v produkčním prostředí. V takovém případě má `Seed` metoda k vložení do databáze pouze data, která má být vložen do produkčního prostředí. Například může být vhodné zahrnout názvy skutečné oddělení v databázi `Department` tabulky, když se aplikace zpřístupní v produkčním prostředí.

Pro účely tohoto kurzu budete používat migrace pro nasazení, ale vaše `Seed` metoda bude přesto vložit testovací data pro snazší zjistit, jak funguje funkčnost aplikace, aniž byste museli ručně vložit velké množství dat.

1. Nahraďte obsah *Configuration.cs* souboru následujícím kódem, který načte testovací data do nové databáze. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [Počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda přijímá jako vstupní parametr objekt kontextu databáze, a kód v metodě používá tento objekt k přidání nové entity do databáze. Pro každý typ entity kód vytvoří kolekci nové entity, se přidají do příslušné [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) vlastnost a uloží změny do databáze. Není nutné volat [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda po každé skupiny entit, jako se zde provádí, ale způsobem, který pomáhá při hledání příčiny problému, pokud dojde k výjimce, zatímco kód zapisuje do databáze.

    Některé příkazy, které vkládají data používají [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody k provedení operace "upsert". Vzhledem k tomu, `Seed` metoda pracuje s každou migraci, data, nelze vložit stejně, protože se snažíte přidat řádky už budou tam po první migrace, který vytvoří databázi. Operace "upsert", zabraňuje chybám, které by mohlo dojít, pokud se pokusíte vložit řádek, který již existuje, ale ***přepíše*** všechny změny dat, který může mít provedené při testování aplikace. Pomocí testovacích dat v některých tabulkách nechcete provést: v některých případech při změně dat při testování chcete své změny zůstat po aktualizacích databáze. V takovém případě ji chcete vložit podmíněné operace: Vložit řádek jenom v případě, že ještě neexistuje. Seed – metoda používá oba přístupy.

    První parametr předána [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metody určuje vlastnost, která má použít ke kontrole, jestli řádek již existuje. Pro testovací data studentů, které poskytujete `LastName` vlastnost lze použít pro tento účel, protože je jedinečný název každé poslední v seznamu:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Tento kód předpokládá, že poslední názvy musí být jedinečné. Pokud chcete ručně přidat studenta s duplicitním názvem poslední, zobrazí se následující výjimka při příštím provedení migrace.

    Posloupnost obsahuje více než jeden prvek.

    Další informace o `AddOrUpdate` metodu, najdete v článku [postará metodou AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

    Kód, který přidá `Enrollment` nepoužívá entity `AddOrUpdate` metody. Zkontroluje, jestli entita již existuje a vloží entitu, pokud neexistuje. Tento přístup se zachovat změny, které provedete na podnikové úrovni registraci při spuštění migrace. Kód prochází každý člen `Enrollment` [seznamu](https://msdn.microsoft.com/library/6sh2ey19.aspx) a pokud registrace nebyl nalezen v databázi, přidá přihlášení k databázi. Při prvním aktualizaci databáze, databáze se bude prázdný, tak přidá každý registrace.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Informace o tom, jak ladit `Seed` metoda a zpracování redundantních dat, jako je například dvou studentů s názvem "Alexander" carson, ředitelka najdete v části [financování a ladění Entity Framework (EF) databází](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Ricka Andersona.
2. Sestavte projekt.

### <a name="create-and-execute-the-first-migration"></a>Vytvoření a provedení první migrace

1. V okně konzoly Správce balíčků zadejte následující příkazy: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Příkaz se přidá do složky při přenesení *[DateStamp]\_InitialCreate.cs* soubor, který obsahuje kód, který vytvoří databázi. První parametr (`InitialCreate)` se používá pro soubor pojmenujte a může být cokoliv, co chcete; obvykle zvolíte slovo nebo slovní spojení, které shrnuje, co se provádí v migraci. Můžete například pojmenovat pozdější migraci &quot;AddDepartmentTable&quot;.

    ![Složka migrace s počáteční migraci](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up` Metodu `InitialCreate` třída vytvoří databázové tabulky, které odpovídají sady entit datového modelu a `Down` je metoda odstraní. Migrace volání `Up` metody k implementaci změn datových modelů pro migraci. Po zadání příkazu vrácení zpět aktualizace, volání migrace `Down` metody. Následující kód zobrazí obsah `InitialCreate` souboru:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` Příkaz spustí `Up` spustí metodu pro vytvoření databáze a pak ho `Seed` metoda k naplnění databáze.

Databáze serveru SQL Server byla vytvořena pro datový model. Název databáze je *ContosoUniversity*a *.mdf* soubor je ve vašem projektu *aplikace\_Data* složky vzhledem k tomu, který je zadán v vaší připojovací řetězec.

Můžete použít buď **Průzkumníka serveru** nebo **Průzkumník objektů systému SQL Server** (SSOX) Chcete-li zobrazit databáze v sadě Visual Studio. Pro účely tohoto kurzu budete používat **Průzkumníka serveru**. V aplikaci Visual Studio Express 2012 pro Web **Průzkumníka serveru** nazývá **Průzkumník databáze**.

1. Z **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.
2. Klikněte na tlačítko **přidat připojení** ikonu.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Pokud se zobrazí výzva s **zvolit zdroj dat** dialogového okna, klikněte na tlačítko **Microsoft SQL Server**a potom klikněte na tlačítko **pokračovat**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. V **přidat připojení** dialogového okna zadejte **(localdb) \v11.0** pro **název serveru**. V části **vyberte nebo zadejte název databáze**vyberte **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Klikněte na tlačítko **OK.**
6. Rozbalte **SchoolContext** a potom rozbalte **tabulky**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Klikněte pravým tlačítkem na **Student** tabulky a klikněte na tlačítko **zobrazit Data tabulky** zobrazit sloupce, které byly vytvořeny a řádky, které byly vloženy do tabulky.

    ![Tabulka Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Vytvoření Kontroleru studentů a zobrazení

Dalším krokem je vytvoření kontroleru a zobrazení ASP.NET MVC ve vaší aplikaci, můžete pracovat s některou z těchto tabulek.

1. K vytvoření `Student` klikněte pravým tlačítkem na kontroler, **řadiče** složky v **Průzkumníku řešení**vyberte **přidat**a potom klikněte na **Kontroleru** . V **přidat kontroler** dialogového okna zadejte následující volby a pak klikněte na tlačítko **přidat**: 

   - Název kontroleru: **StudentController**.
   - Šablona: **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s využitím rozhraní Entity Framework**.
   - Třída modelu: **Student (ContosoUniversity.Models)**. (Pokud se tato možnost v rozevíracím seznamu nezobrazí, sestavte projekt a zkuste to znovu.)
   - Třída kontextu dat: **SchoolContext (ContosoUniversity.Models)**.
   - Zobrazení: **Razor (CSHTML)**. (Výchozí nastavení.)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio otevře *Controllers\StudentController.cs* souboru. Uvidíte, že proměnné třídy se vytvořil, který vytvoří instanci objektu kontextu databáze:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index` Metody akce získá seznam studenti z *studenty* načtením sadu entit `Students` vlastnost instance kontextu databáze:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml* zobrazení seznamu v tabulce:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Stiskněte kombinaci kláves CTRL + F5 ke spuštění projektu.

     Klikněte na tlačítko **studenty** kartu pro zobrazení testovacích dat, který `Seed` metoda vložen.

     ![Student indexová stránka](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Konvence

Množství kódu, které jste měli pro zápis v pořadí pro Entity Framework umožnit vytvoření kompletní databáze je minimální, protože se používá *konvence*, nebo předpokladů, které díky rozhraní Entity Framework. Některé z nich už byly zjištěny:

- Pluralized formy názvy tříd entit se používají jako názvy tabulek.
- Názvy vlastností entity se používají pro názvy sloupců.
- Vlastnosti entity, které jsou pojmenovány `ID` nebo *classname* `ID` jsou rozpoznány jako vlastnosti primárního klíče.

Viděli jsme, že se dá přepsat konvence (například jste zadali, že názvy tabulek by neměl být pluralized), a získáte další informace o vytváření a jak je v přepsat [vytváření více komplexní datový Model](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) kurz později v této sérii. Další informace najdete v tématu [první konvence kódu](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Souhrn

Právě jste vytvořili jednoduchou aplikaci, která se používá k ukládání a zobrazení dat Entity Framework a systému SQL Server Express. V následujícím kurzu dozvíte jak provést základní CRUD (vytváření, čtení, aktualizace nebo odstranění) operace. Zpětná vazba v dolní části této stránky můžete nechat. Dejte nám prosím vědět jak líbilo tuto část kurzu a jak jsme mohli vylepšit.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
