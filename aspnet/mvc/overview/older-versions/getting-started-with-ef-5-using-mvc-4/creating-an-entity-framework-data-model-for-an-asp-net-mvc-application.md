---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Vytvoření datového modelu Entity Framework pro aplikaci ASP.NET MVC (1 10) | Microsoft Docs
author: tdykstra
description: Je k dispozici pro Visual Studio 2013, Entity Framework 6 a MVC 5 novější verze tohoto kurzu řady. Contoso univerzity ukázkové webové aplikace de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a963f26b408f2a54bd9cd3e852bc1e368f86c41f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877928"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Vytvoření datového modelu Entity Framework pro aplikaci ASP.NET MVC (1 10)
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A [novější verze tohoto kurzu řady](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) je k dispozici pro Visual Studio 2013, Entity Framework 6 a MVC 5.
> 
> 
> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 a Visual Studio 2012. Vzorová aplikace je web pro fiktivní vysoké školy Contoso. Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem. Tento kurz řady vysvětluje, jak vytvořit Contoso univerzity ukázkovou aplikaci. Můžete [stáhnout hotová aplikace](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Nejprve kódu
> 
> Existují tři způsoby, můžete pracovat s daty v Entity Framework: *Database First*, *Model First*, a *Code First*. Tento kurz je určen pro Code First. Informace o rozdílech mezi tyto pracovní postupy a pokyny o tom, jak zvolit tu nejvhodnější pro váš scénář najdete v tématu [Entity Framework vývoj pracovních](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Ukázkové aplikace je založený na [ASP.NET MVC](../../../index.md). Pokud dáváte přednost práci s modelem webových formulářů ASP.NET, najdete v článku [vazby modelu a webové formuláře](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) kurz řady a [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> | **Zobrazí v tomto kurzu** | **Taky spolupracuje se službou** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express pro Web. To je automaticky nainstalován aplikací Windows Azure SDK Pokud ještě nemáte VS 2012 a VS 2012 Express pro Web. Visual Studio 2013 by měla fungovat, ale tento kurz nebyl testován s ním a některé možnosti nabídky a dialogových oknech se liší. [VS 2013 verze sady Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) je vyžadována pro nasazení systému Windows Azure. |
> | .NET 4.5 | Většina funkcí zobrazí bude fungovat v rozhraní .NET 4, ale některé nebude. Například výčtu podpory v EF vyžaduje .NET 4.5. |
> | Rozhraní Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Pokud přeskočíte kroky nasazení systému Windows Azure, nepotřebujete sady SDK. Po vydání nové verze sady SDK, bude odkaz nainstalujte novější verzi. V takovém případě budete muset přizpůsobit některé pokyny k nové uživatelské rozhraní a funkcí. |
> 
> ## <a name="questions"></a>Otázky
> 
> Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx), [Entity Framework a technologie LINQ to Entities fórum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), nebo [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Potvrzování
> 
> Zobrazit poslední kurz v řadě pro [potvrzování a Poznámka o VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Původní verze kurzu
> 
> Je k dispozici v původní verzi tohoto kurzu [EF 4.1 nebo elektronická kniha MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Contoso univerzity webové aplikace

Aplikace, kterou jste budete vytvářet v těchto kurzech je jednoduchý univerzity webovou stránku.

Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace. Zde najdete několik z obrazovky, které vytvoříte.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Styl uživatelského rozhraní tohoto webu byla choval blízko co je generován integrované šablony tak, aby tento kurz můžete zaměřit především na tom, jak používat rozhraní Entity Framework.

## <a name="prerequisites"></a>Požadavky

Pokyny a snímky obrazovky v tomto kurzu předpokládá, že používáte [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) nebo [Visual Studio 2012 Express pro Web](https://go.microsoft.com/fwlink/?LinkID=275131), nejnovější aktualizace a Azure SDK for .NET nainstalované od července, 2013. Můžete získat všechny tyto se na následující odkaz:

[Azure SDK pro platformu .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Pokud máte nainstalovanou sadu Visual Studio, výše uvedený odkaz nainstaluje všechny chybějící součásti. Pokud nemáte Visual Studio, odkaz nainstaluje Visual Studio 2012 Express pro Web. Můžete použít Visual Studio 2013, ale některé požadované procedury a obrazovky, se budou lišit.

## <a name="create-an-mvc-web-application"></a>Vytvořit webovou aplikaci MVC

Otevřete Visual Studio a vytvořte nový projekt C# s s názvem "ContosoUniversity" pomocí **webové aplikace ASP.NET MVC 4** šablony. Zajistěte, aby cílíte **rozhraní .NET Framework 4.5** (které budete používat [ `enum` vlastnosti](https://msdn.microsoft.com/data/hh859576.aspx), a který vyžaduje .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

V **nový ASP.NET MVC 4 projekt** dialogovém okně vyberte pole **Internetové aplikace** šablony.

Ponechte **Razor** zobrazit vybraný modul a nechat **vytvoření projektu testování částí** políčko nezaškrtnuté.

Click **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Nastavení stylu lokality

Pár jednoduchými změnami nastaví lokality nabídky, rozložení a domovské stránky.

Otevřete *Views\Shared\\_Layout.cshtml*a obsah souboru nahraďte následujícím kódem. Změny se zvýrazněnou.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Tento kód provede tyto změny:

- Nahradí instance šablony "Moje aplikace technologie ASP.NET MVC" a "zde bude vaše logo" "Univerzity Contoso".
- Přidá několik odkazy na akce, které se použijí později v tomto kurzu.

V *Views\Home\Index.cshtml*, nahraďte obsah souboru následující kód k vyloučení odstavců šablony o ASP.NET a MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

V *Controllers\HomeController.cs*, změňte hodnotu `ViewBag.Message` v `Index` metody akce k "Vítá vás Contoso univerzity!", jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Stiskněte kombinaci kláves CTRL + F5 spusťte web. Zobrazí domovská stránka s v hlavní nabídce.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Vytvoření datového modelu

Dále vytvoříte tříd entit pro danou aplikaci univerzity Contoso. Budete začínat následující tři entity:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Je vztah jeden mnoho mezi `Student` a `Enrollment` entity, a je mezi vztah jeden mnoho `Course` a `Enrollment` entity. Jinými slovy student může být zaregistrované v libovolný počet kurzy a kurzu může mít libovolný počet studenti, kteří jsou v ní zaregistrované.

V následujících částech vytvoříte třídu pro každé z nich o těchto entitách.

> [!NOTE]
> Pokud se pokusíte kompilace projektu, dokud nedokončíte všechny tyto entity třídy vytváření, získáte chyby kompilátoru.


### <a name="the-student-entity"></a>Student Entity

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

V *modely* složku vytvořit *Student.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` Vlastnost bude sloupec primárního klíče tabulky databáze, která odpovídá této třídy. Ve výchozím nastavení, rozhraní Entity Framework interpretuje vlastnost s názvem `ID` nebo *classname* `ID` jako primární klíč.

`Enrollments` Vlastnost je *navigační vlastnost*. Navigační vlastnosti podržte dalšími subjekty, které se vztahují k této entity. V takovém případě `Enrollments` vlastnost `Student` entity bude obsahovat všechny `Enrollment` entit, které se vztahují k které `Student` entity. Jinými slovy pokud dané `Student` řádků v databázi má dva související `Enrollment` řádků (hodnota řádky, které obsahují tento Studentova primární klíč v jejich `StudentID` sloupec cizího klíče), že `Student` entity `Enrollments` navigační vlastnost bude obsahovat tyto dvě `Enrollment` entity.

Navigační vlastnosti jsou obvykle definovány jako `virtual` tak, aby mohou využít určitých funkcí rozhraní Entity Framework, jako *opožděného načítání*. (Vysvětlení opožděného načítání naleznete dále v [související Data čtení](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurz později z této série.

Pokud vlastnost navigace mohou být uloženy více entit (jako relace m: n nebo na více), jeho typ musí být seznam, ve kterém položky možné ji přidat, odstranit nebo aktualizovat, například `ICollection`.

### <a name="the-enrollment-entity"></a>Registrace Entity

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

V *modely* složku vytvořit *Enrollment.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Vlastnost úrovni je [výčtu](https://msdn.microsoft.com/data/hh859576.aspx). Otazník po `Grade` deklaraci typu znamená, že `Grade` vlastnost je [s možnou hodnotou Null](https://msdn.microsoft.com/library/2cf62fcy.aspx). Třída, která má hodnotu null se liší od nulové úrovni – hodnota null znamená úrovni není známý nebo ještě nebyly přiřazeny.

`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Student`. `Enrollment` Entita je spojen s jednou `Student` entit, tak vlastnost mohou obsahovat pouze jeden `Student` entity (na rozdíl od `Student.Enrollments` navigační vlastnost jste viděli dříve, která může pojmout více `Enrollment` entity).

`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Course`. `Enrollment` Entita je spojen s jednou `Course` entity.

### <a name="the-course-entity"></a>Během Entity

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

V *modely* složku vytvořit *Course.cs*, existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` Je navigační vlastnost. A `Course` entity může souviset s libovolný počet `Enrollment` entity.

Budete říkáme, informace o [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Žádný)] atribut v dalším kurzu. V zásadě platí tento atribut slouží k zadání primární klíč pro během, místo aby databázi jeho vygenerování.

## <a name="create-the-database-context"></a>Vytvoření kontextu databáze

Hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model je *kontext databáze* třídy. Vytvořit této třídy odvozené z [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) třídy. V kódu zadáte entit, které jsou zahrnuty v datovém modelu. Můžete také upravit chování určité Entity Framework. V tomto projektu je třída s názvem `SchoolContext`.

Vytvořte složku s názvem *DAL* (pro Data Access Layer). V této složce vytvořte soubor novou třídu s názvem *SchoolContext.cs*a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Tento kód vytvoří [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) vlastnosti pro každou sadu entit. V terminologii rozhraní Entity Framework *sady entit* obvykle odpovídá do databázové tabulky a *entity* odpovídá na řádek v tabulce.

`modelBuilder.Conventions.Remove` Příkaz v [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda zabrání se pluralized názvy tabulek. Pokud nebylo to uděláte, by generovaného tabulky s názvem `Students`, `Courses`, a `Enrollments`. Místo toho budou názvy tabulek `Student`, `Course`, a `Enrollment`. Vývojáři Nesouhlasím o tom, jestli by měl názvy tabulek pluralized nebo ne. Tento kurz používá jednotném čísle, ale důležité je, že můžete vybrat libovolného tvaru dáváte přednost zahrnutím nebo vynechání tento řádek kódu.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) je Odlehčená verze SQL Server Express databázový stroj, který se spustí na vyžádání a běží v uživatelském režimu. LocalDB běží v režimu speciální spuštění systému SQL Server Express, který umožňuje pracovat s databázemi jako *.mdf* soubory. Obvykle jsou zachovány soubory databáze LocalDB v *aplikace\_Data* složky webového projektu. Instance funkce uživatel v systému SQL Server Express také umožňuje pracovat s *.mdf* soubory, ale funkce instance uživatel je zastaralý, tedy LocalDB doporučuje se pro práci s *.mdf* soubory.

Obvykle SQL Server Express se nepoužije pro provozní webové aplikace. LocalDB zejména se nedoporučuje pro produkční použití s webovou aplikací protože není určeno k práci se službou IIS.

V sadě Visual Studio 2012 a novějších verzích je ve výchozím nastavení pomocí sady Visual Studio nainstalována LocalDB. V sadě Visual Studio 2010 a dřívějších verzích SQL Server Express (bez LocalDB) je nainstalována ve výchozím nastavení pomocí sady Visual Studio; budete muset ručně ji nainstalovat, pokud používáte Visual Studio 2010.

V tomto kurzu budete pracujete s LocalDB tak, aby databáze mohou být uloženy ve *aplikace\_Data* složky jako *.mdf* souboru. Otevřete kořenu *Web.config* souboru a přidat nový připojovací řetězec k `connectionStrings` kolekce, jak je znázorněno v následujícím příkladu. (Zajistěte, aby je aktualizovat *Web.config* soubor v kořenové složce projektu. K dispozici je také *Web.config* soubor *zobrazení* podsložky, která není nutné aktualizovat.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Ve výchozím nastavení, hledá rozhraní Entity Framework se stejným názvem jako připojovací řetězec `DbContext` – třída (`SchoolContext` pro tento projekt). Určuje připojovací řetězec, který jste přidali LocalDB databáze s názvem *ContosoUniversity.mdf* umístěný v *aplikace\_Data* složky. Další informace najdete v tématu [připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Nemusíte ve skutečnosti zadejte připojovací řetězec. Pokud nezadáte připojovací řetězec, rozhraní Entity Framework vytvoří za vás; však nemusí být databáze v *aplikace\_data* složky vaší aplikace. Informace, na kterém se vytvoří databáze najdete v tématu [Code First pro novou databázi](https://msdn.microsoft.com/data/jj193542).

`connectionStrings` Kolekce má také připojovací řetězec s názvem `DefaultConnection` sloužící pro databázi členství. Nebudete používat databázi členství v tomto kurzu. Jediným rozdílem mezi dva připojovací řetězce je název databáze a hodnota atributu název.

## <a name="set-up-and-execute-a-code-first-migration"></a>Nastavení a provedení migrace první kódu

Při prvním spuštění nástroje pro vývoj aplikací, datový model změn často a pokaždé, když změn modelu, které získá synchronizována s databází. Můžete nakonfigurovat automaticky vyřadit a znovu vytvořit databázi při každé změně datového modelu Entity Framework. To není problém již v rané fázi ve vývoji, protože je snadno znovu vytvořit testovací data, ale po nasazení do produkčního prostředí obvykle chcete aktualizovat schéma databáze bez jejich odstranění databáze. Povolí funkci migrace Code First k aktualizaci databáze bez vyřadit a znovu ji vytvořit. Časná v cyklu vývoje nového projektu můžete chtít použít [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) vyřadit, znovu vytvořte a znovu naplnit databázi pokaždé, když změny modelu. Jeden získáte připraveny k nasazení aplikace, můžete převést na metodu migrace. Pro účely tohoto kurzu budete používat jenom migrace. Další informace najdete v tématu [migrace Code First](https://msdn.microsoft.com/data/jj591621) a [migrace záznam dění na monitoru řady](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Povolení migrace Code First

1. Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Na `PM>` řádku zadejte následující příkaz:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![příkaz enable-migrations se](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Tento příkaz vytvoří *migrace* složky v projektu ContosoUniversity a uloží je v této složce *Configuration.cs* soubor, který můžete upravit konfigurace migrací.

    ![Složku migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration` Obsahuje třídy `Seed` metoda, která je volána při vytvoření databáze a pokaždé, když je aktualizován po datový model změnit.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Účelem tohoto `Seed` metoda je umožnit testovací data po Code First ji vytvoří nebo aktualizuje ji vložit do databáze.

### <a name="set-up-the-seed-method"></a>Nastavit Seed – metoda

[Počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda se spouští při migrace Code First vytvoří databázi a pokaždé, když se aktualizuje databázi nejnovější migrace. Účelem metodu počáteční hodnoty je umožnit vložení dat do tabulek před aplikace přistupovat k databázi poprvé.

V dřívějších verzích systému Code First, před migrací vydáním, bylo běžné `Seed` metody vložit testovací data, protože při každé změně modelu během vývoje neprobíhala v databázi úplně odstranit a znovu vytvořit od začátku. Pomocí migrace Code First, testovací data se uchovávají po provedení změn databáze, takže včetně testovací data v [počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda není obvykle nutné. Ve skutečnosti, které nechcete `Seed` metoda vložit testovací data, pokud budete používat migrace pro nasazení databáze do produkčního prostředí, protože `Seed` metoda bude spuštěna v produkčním prostředí. V takovém případě má `Seed` metoda vložit do databáze pouze data, která má být vložen v produkčním prostředí. Například můžete zahrnout názvy skutečné oddělení v databázi `Department` tabulky, až aplikace k dispozici v produkčním prostředí.

V tomto kurzu budete používat migrace pro nasazení, ale vaše `Seed` metoda bude přesto vložit testovací data pro snazší zjistit, jak funguje funkce aplikace, aniž by bylo nutné ručně vložit velké množství dat.

1. Nahraďte obsah *Configuration.cs* soubor s následující kód, který načte testovací data do nové databáze. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [Počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda přebírá objekt kontextu databáze jako vstupní parametr a kód v metodě používá tento objekt k přidání nové entity do databáze. Ke každému typu entity kód vytvoří kolekci nové entity, se přidají do příslušné [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) vlastnost a uloží změny do databáze. Není nutné volat [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda po každé skupiny entit, jako je na jednom místě, ale umožňuje učinit vyhledejte zdroj problému, pokud dojde k výjimce při kód je zápis do databáze.

    Některé příkazy, který vkládá data pomocí [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda o provedení operace "upsert". Protože `Seed` metoda se spouští se každých migrace, data, nelze právě vložit, protože řádky, které se pokoušíte přidat bude po první migrace, která vytváří databázi již existovat. Operace "upsert", zabraňuje chyb, které by mohlo dojít, pokud se pokusíte vložit řádek, který již existuje, ale ***přepsání*** všechny změny dat, která může provedení při testování aplikace. Pro testovací data v některé tabulky nemusí Chcete to tak proběhlo: v některých případech při změně dat při testování vaše změny zůstat po aktualizace databáze. V takovém případě budete chtít provést operace podmíněného insert: Vložit řádek pouze v případě, že ještě neexistuje. Metoda počáteční hodnoty pomocí obou přístupů.

    První parametr předaný [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda určuje vlastnost, která má použít pro kontrolu, pokud řádek již existuje. Pro testovací data studenty, který zadáte `LastName` vlastnost lze použít pro tento účel, protože každý příjmení v seznamu je jedinečný:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Tento kód předpokládá, že poslední názvy jsou jedinečné. Pokud chcete ručně přidat student s duplicitním názvem poslední, získáte následující výjimka při příštím provedení migrace.

    Sekvence obsahuje více než jeden element.

    Další informace o `AddOrUpdate` metodu, najdete v části [postará s EF 4.3 AddOrUpdate metoda](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) na blogu Julie Lerman.

    Kód, který přidá `Enrollment` entity nepoužívá `AddOrUpdate` metoda. Zkontroluje, zda entita již existuje a vloží entitu, pokud neexistuje. Tento přístup se zachovat změny, které můžete provést úrovni registrace při spuštění migrace. Kód prochází každý člen `Enrollment` [seznamu](https://msdn.microsoft.com/library/6sh2ey19.aspx) a pokud registrace nebyla nalezena v databázi, přidá zápisu do databáze. Při prvním aktualizaci databáze, databáze se bude prázdný, proto přidá každou registrace.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Informace o tom, jak ladit `Seed` metoda a určuje způsob zpracování redundantních dat, jako je například dvou studentů s názvem "Alexander Carsonovy", najdete v části [první financování a ladění Entity Framework (EF) databází](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) na blogu Ricka Andersona.
2. Sestavte projekt.

### <a name="create-and-execute-the-first-migration"></a>Vytvoření a provedení první migrace

1. V okně konzoly Správce balíčků zadejte následující příkazy: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Příkaz přidá do složky, migrace *[DateStamp]\_InitialCreate.cs* soubor, který obsahuje kód, který vytvoří databázi. První parametr (`InitialCreate)` se používá pro soubor název a může být všechno; obvykle zvolíte slovo nebo frázi, která shrnuje, co probíhá při migraci. Například můžete třeba pojmenovat novější migrace &quot;AddDepartmentTable&quot;.

    ![Migrace složku s počáteční migrace](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up` Metodu `InitialCreate` třída vytvoří databázové tabulky, které odpovídají sady dat modelu entity a `Down` je metoda odstraní. Migrace volání `Up` metody k implementaci změny modelu dat pro migraci. Když zadáte příkaz k vrácení aktualizace, migrace volání `Down` metoda. Následující kód ukazuje obsah `InitialCreate` souboru:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` Příkaz spustí `Up` metodu pro vytvoření databáze a pak se spustí `Seed` metoda k naplnění databáze.

Databázi systému SQL Server byla vytvořena pro datový model. Název databáze je *ContosoUniversity*a *.mdf* soubor je ve vašem projektu *aplikace\_Data* složky protože se jedná, co jste zadali v vaší připojovací řetězec.

Můžete použít buď **Průzkumníka serveru** nebo **Průzkumník objektů systému SQL Server** (SSOX) Chcete-li zobrazit databázi v sadě Visual Studio. Pro účely tohoto kurzu budete používat **Průzkumníka serveru**. V sadě Visual Studio Express 2012 pro Web **Průzkumníka serveru** nazývá **Průzkumník databáze**.

1. Z **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.
2. Klikněte **přidat připojení** ikonu.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Pokud se zobrazí výzva s **zvolit zdroj dat** dialogové okno, klikněte na tlačítko **Microsoft SQL Server**a potom klikněte na **pokračovat**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. V **přidat připojení** dialogovém okně zadejte **(localdb) \v11.0** pro **název serveru**. V části **vyberte nebo zadejte název databáze**, vyberte **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Klikněte na tlačítko **OK.**
6. Rozbalte položku **SchoolContext** a potom rozbalte **tabulky**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Klikněte pravým tlačítkem myši **Student** tabulky a klikněte na tlačítko **zobrazit Data tabulky** sloupce, které byly vytvořeny a řádky, které byly vloženy do tabulky.

    ![Tabulka Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Probíhá vytváření řadič Student a zobrazení

Dalším krokem je vytvoření kontroleru a zobrazení ASP.NET MVC v aplikaci, které můžete pracovat s některou z těchto tabulek.

1. K vytvoření `Student` řadiče, klikněte pravým tlačítkem myši **řadiče** složky v **Průzkumníku řešení**, vyberte **přidat**a potom klikněte na **řadiče** . V **přidat kontroler** dialogové okno pole, vyberte následující možnosti a pak klikněte na tlačítko **přidat**: 

   - Název řadiče: **StudentController**.
   - Šablona: **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s využitím nástroje Entity Framework**.
   - Třída modelu: **Student (ContosoUniversity.Models)**. (Pokud tato možnost v rozevíracím seznamu nezobrazí, sestavte projekt a zkuste to znovu.)
   - Třída kontextu dat: **SchoolContext (ContosoUniversity.Models)**.
   - Zobrazení: **Razor (CSHTML)**. (Výchozí nastavení.)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Otevře se Visual Studio *Controllers\StudentController.cs* souboru. Uvidíte, že byl vytvořen proměnné třídy, který vytvoří instanci objektu kontextu databáze:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index` Metoda akce získá seznam studenty z *studenty* načtením sadu entit `Students` vlastností kontextu instance databáze:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml* zobrazení zobrazí tento seznam v tabulce:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Stisknutím kombinace kláves CTRL + F5 a spusťte projekt.

     Klikněte na tlačítko **studenty** karty zobrazíte testovací data, `Seed` metoda vložit.

     ![Student indexovou stránku](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Konvence

Kvůli použití je minimální množství kódu, které jste měli k zápisu, aby rozhraní Entity Framework, abyste mohli vytvořit databázi dokončení pro vás *konvence*, nebo předpoklady, které umožňuje Entity Framework. Některé z nich již byly zjištěny:

- Pluralized formy názvy tříd entity se používají jako názvy tabulek.
- Názvy vlastností entity se používají pro názvy sloupců.
- Vlastnosti entity, které jsou s názvem `ID` nebo *classname* `ID` jsou rozpoznán jako vlastnosti primárního klíče.

Už víte, že je možné přepsat konvence (například jste určili, že by neměl být pluralized názvy tabulek), a dozvíte další informace o konvence a postupu přepsání je v [vytváření další komplexní Model dat](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) kurzu dále v této série. Další informace najdete v tématu [první pravidla týkající se kódu](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Souhrn

Nyní jste vytvořili jednoduchou aplikaci, která se používá k uložení a zobrazení data Entity Framework a SQL Server Express. V následujícím kurzu se dozvíte jak provádět základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) operace. Můžete ponechat zpětná vazba v dolní části této stránky. Dejte nám vědět, jak můžete tuto část kurzu líbilo a jak jsme může vylepšit.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
