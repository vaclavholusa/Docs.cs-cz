---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Začínáme s Entity Framework 6 Code First pomocí MVC 5 | Dokumentace Microsoftu
author: tdykstra
description: 'Je k dispozici novější verze této sérii: Začínáme s ASP.NET Core a Entity Framework Core pomocí sady Visual Studio 2015. Contoso Universi...'
ms.author: aspnetcontent
ms.date: 10/22/2015
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f03ddcf7dcc8b5d20c5459a7fb0015ab20f340c5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837166"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Začínáme s Entity Framework 6 Code First pomocí MVC 5
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stahovat PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Je k dispozici novější verze této sérii: [Začínáme s ASP.NET Core a Entity Framework Core pomocí sady Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 5 pomocí Entity Framework 6 a Visual Studio 2013. Tento kurz používá Code First pracovního postupu. Informace o tom, jak si vybrat mezi Code First, Database First a první Model, najdete v části [Entity Framework vývojářské pracovní postupy](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> Ukázková aplikace je webovou stránku pro fiktivní společnosti Contoso University. Zahrnuje funkce, jako student přijetí, kurz vytvoření a přiřazení instruktorem. Tato série kurzů vysvětluje, jak vytvořit ukázková aplikace Contoso University. Je možné [stáhnout hotovou aplikaci](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Verze jazyka Visual Basic přeložený Mike Brind je k dispozici: [MVC 5 s EF 6 v jazyce Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting lokality.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (balíček NuGet EntityFramework 6.1.0)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (volitelné)
>   
> 
> Tento kurz, mělo by také fungovat s [Visual Studio 2013 Express for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) nebo Visual Studio 2012. [VS 2012 verzi sady Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) je vyžadován pro nasazení Windows Azure pomocí sady Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Kurz verze
> 
> Předchozí verze tohoto kurzu, najdete v části [EF 4.1 / MVC 3 e kniha](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) a [Začínáme s EF 5 pomocí MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Otázky a komentáře
> 
> Napište prosím zpětnou vazbu o tom, jak vám líbilo v tomto kurzu a co můžeme zlepšit v komentářích v dolní části stránky. Pokud máte nějaké otázky, které přímo nesouvisejí, najdete v tomto kurzu, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework a LINQ to Entities fórum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), nebo [ StackOverflow.com](http://stackoverflow.com/).
> 
> Pokud narazíte na problém, který nelze přeložit, obecně najdete řešení problému porovnáním kód dokončený projekt, který si můžete stáhnout. Některé běžné chyby a jejich řešení najdete v tématu [běžné chyby a řešení či alternativní řešení pro ně.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Webová aplikace Contoso University

Aplikace, kterou je budete vytvářet v těchto kurzech je webová stránka jednoduché university.

Uživatelé mohou zobrazit a aktualizovat Všichni studenti, kurz a informace instruktorem. Tady je několik obrazovek, které vytvoříte.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Upravit studenta](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Styl uživatelského rozhraní tohoto webu má blízko co je generována pomocí integrovaných šablon, ukládají, abyste se mohli zaměřit kurz hlavně na tom, jak používat rozhraní Entity Framework.

## <a name="prerequisites"></a>Požadavky

Zobrazit **verze softwaru** v horní části stránky. Entity Framework 6 není požadována, protože nainstalovat balíček EF NuGet v rámci tohoto kurzu.

## <a name="create-an-mvc-web-application"></a>Vytvořit webovou aplikaci MVC

Otevřít Visual Studio a vytvořte nový projekt C# Web s názvem "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

V **nový projekt ASP.NET** dialogové okno Vyberte **MVC** šablony.

Pokud **hostovat v cloudu** zaškrtávací políčko **Microsoft Azure** vybraný oddíl, vymažte ho.

Klikněte na tlačítko **změnit ověřování**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

V **změna ověřování** dialogu **bez ověřování**a potom klikněte na tlačítko **OK**. Pro účely tohoto kurzu nebudete mít by uživatelé museli přihlásit nebo omezení přístupu na základě, na který je přihlášen.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Zpět v dialogovém okně Nový projekt ASP.NET, klikněte na tlačítko **OK** pro vytvoření projektu.

## <a name="set-up-the-site-style"></a>Nastavit styl lokality

Několik jednoduchých změn se nastavit v nabídce webu, rozložení a domovské stránky.

Otevřít *Views\Shared\\_Layout.cshtml*a proveďte následující změny:

- Změňte všechny výskyty "My ASP.NET Application" a "Název aplikace" na "University společnosti Contoso".
- Přidání položek nabídky pro studenty, kurzy, vyučující a oddělení a odstraňte záznam kontaktu.

Změny jsou zvýrazněné.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

V *Views\Home\Index.cshtml*, nahraďte obsah souboru následující kód, který nahradí text o ASP.NET a MVC o této aplikaci:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Stisknutím klávesy CTRL + F5 pro spuštění tohoto webu. Zobrazí domovská stránka s hlavní nabídky.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Nainstalujte rozhraní Entity Framework 6

Z **nástroje** klikněte na nabídku **Správce balíčků NuGet** a potom klikněte na tlačítko **Konzola správce balíčků**.

V **Konzola správce balíčků** okně zadejte následující příkaz:

`Install-Package EntityFramework`

![EF nainstalovaný](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Na obrázku 6.0.0 instalován, ale NuGet nainstaluje nejnovější verzi rozhraní Entity Framework (s výjimkou předběžných verzí), která od nejnovější aktualizaci najdete v tomto kurzu je 6.1.1.

Tento krok je jedním z několika kroků, že se v tomto kurzu, můžete provést ručně, ale které by mohly jsme udělali automaticky funkcí generování uživatelského rozhraní ASP.NET MVC. Tímto způsobem je ručně tak, abyste viděli kroky potřebné k použití rozhraní Entity Framework. Později budete používat generování uživatelského rozhraní pro vytvoření kontroleru MVC a zobrazení. Alternativou je umožnit generování uživatelského rozhraní automaticky nainstalovat balíček EF NuGet, vytvořit třídy kontextu databáze a vytvořit připojovací řetězec. Jakmile budete připraveni to udělat tak, je vše, co musíte udělat Přeskočit tyto kroky a generování uživatelského rozhraní řadiče MVC po vytvoření tříd entit.

## <a name="create-the-data-model"></a>Vytvoření datového modelu

Dále vytvoříte tříd entit pro aplikaci Contoso University. Začnete s následující tři entity:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Existuje vztah jeden mnoho mezi `Student` a `Enrollment` entity, a existuje vztah jeden mnoho mezi `Course` a `Enrollment` entity. Jinými slovy student možné zaregistrovat libovolný počet kurzy a kurzu může mít libovolný počet studentů zaregistrovaná do něj.

V následujících částech vytvoříte třídu pro každou z těchto entit.

> [!NOTE]
> Pokud se pokusíte ke kompilaci projektu před dokončením vytvoření všech těchto tříd entit, získáte chyby kompilátoru.


### <a name="the-student-entity"></a>Entita studenta

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

V *modely* složce vytvořte soubor třídy *Student.cs* a nahraďte kód šablony následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Vlastnost se stane sloupec primárního klíče tabulky databáze, která odpovídá této třídy. Ve výchozím nastavení interpretuje Entity Framework vlastnost s názvem `ID` nebo *classname* `ID` jako primární klíč.

`Enrollments` Je vlastnost *navigační vlastnost*. Vlastnosti navigace podržte dalšími subjekty, které se vztahují k této entity. V takovém případě `Enrollments` vlastnost `Student` entita bude obsahovat všechny `Enrollment` entity, které se vztahují k, které `Student` entity. Jinými slovy Pokud daný `Student` řádků v databázi má dva související `Enrollment` řádky (hodnota řádky, které obsahují student získal primární klíč v jejich `StudentID` sloupec cizího klíče), který `Student` entity `Enrollments` navigační vlastnost bude obsahovat tyto dvě `Enrollment` entity.

Navigační vlastnosti se obvykle definují jako `virtual` tak, aby se můžete využít některé funkce Entity Framework, jako *opožděné načtení*. (Opožděné načtení budou vysvětlena dále v [čtení souvisejících dat](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) později v této sérii kurzů.)

Pokud vlastnost navigace může obsahovat více entit (jako v relace m: n nebo 1 n), jeho typ musí být seznam, ve kterém položky lze přidávat, odstranit a aktualizovat, například `ICollection`.

### <a name="the-enrollment-entity"></a>Registrace Entity

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

V *modely* složku, vytvořte *Enrollment.cs* a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Bude mít vlastnost primárního klíče; tato entita používá *classname* `ID` vzorku místo `ID` samostatně jako jste viděli v `Student` entity. Obvykle by zvolte jeden model a použít ho v rámci datového modelu. Tady variantu ukazuje, které můžete použít buď vzor. V pozdějších kurzech, zobrazí se vám jak pomocí `ID` bez `classname` usnadňuje implementaci dědičnosti v datovém modelu.

`Grade` Vlastnost je [výčtu](https://msdn.microsoft.com/data/hh859576.aspx). Otazník po `Grade` deklarace typu znamená, že `Grade` vlastnost [s možnou hodnotou Null](https://msdn.microsoft.com/library/2cf62fcy.aspx). Se liší od nulovou na podnikové úrovni na podnikové úrovni, který má hodnotu null – hodnota null znamená známku vyjádřenou není znám nebo ještě nebyly přiřazeny.

`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost je `Student`. `Enrollment` Entita je přidružený nejméně k jednomu `Student` entity, tak vlastnost může obsahovat pouze jeden `Student` entity (na rozdíl od `Student.Enrollments` navigační vlastnost předchozímu příkladu, který může obsahovat více `Enrollment` entity).

`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost je `Course`. `Enrollment` Entita je přidružený nejméně k jednomu `Course` entity.

Nastavení interpretuje Entity Framework vlastnost jako vlastnost cizího klíče Pokud je název *&lt;název navigační vlastnosti&gt;&lt;vlastnost primárního klíče název&gt;* (například `StudentID`pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`). Vlastnosti cizího klíče může také být pojmenován stejně jednoduše *&lt;vlastnost primárního klíče název&gt;* (například `CourseID` od `Course` je primární klíč entity `CourseID`).

### <a name="the-course-entity"></a>Kurz Entity

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

V *modely* složku, vytvořte *Course.cs*, nahraďte kód šablony následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` Je navigační vlastnost. A `Course` entit může souviset s libovolným počtem `Enrollment` entity.

Informace o kliknu [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) atribut v pozdějších kurzech v této sérii. V podstatě tento atribut umožňuje zadat primární klíč pro kurz namísto nutnosti databáze jeho vygenerování.

## <a name="create-the-database-context"></a>Vytvořte kontext databáze

Hlavní třída, která koordinuje funkce Entity Framework pro daný datový model je *kontext databáze* třídy. Vytvoření této třídy odvozené z [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) třídy. V kódu určíte entit, které jsou zahrnuty v datovém modelu. Můžete také přizpůsobit chování určité Entity Framework. V tomto projektu je s názvem třídy `SchoolContext`.

K vytvoření složky v projektu ContosoUniversity, klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** a klikněte na tlačítko **přidat**a potom klikněte na tlačítko **novou složku**. Název nové složky *DAL* (pro Data Access Layer). V této složce vytvořte nový soubor třídy *SchoolContext.cs*a nahraďte kód šablony následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Určení sady entit

Tento kód vytvoří [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) vlastností pro každou sadu entit. V terminologii Entity Framework *sadu entit* obvykle odpovídá tabulku databáze a *entity* odpovídající řádek v tabulce.

> [!NOTE] 
> 
> Může zapomněli `DbSet<Enrollment>` a `DbSet<Course>` příkazy a bude fungovat stejně. Entity Framework bude zahrnovat je implicitně protože `Student` odkazy na entity `Enrollment` entity a `Enrollment` odkazy na entity `Course` entity.


### <a name="specifying-the-connection-string"></a>Zadávání připojovacího řetězce

Název připojovacího řetězce (které přidáte do souboru Web.config později) je předáno konstruktoru.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Můžete také předat samotný připojovací řetězec ne název jednoho, který je uložen v souboru Web.config. Další informace o možnosti pro určení databáze, které se mají použít, najdete v části [Entity Framework – připojení a modely](https://msdn.microsoft.com/data/jj592674).

Pokud nechcete explicitně zadat připojovací řetězec nebo název jednoho, Entity Framework předpokládá, že název připojovacího řetězce je stejný jako název třídy. Výchozí název připojovacího řetězce v tomto příkladu by pak `SchoolContext`, stejně jako co zadáváte explicitně.

### <a name="specifying-singular-table-names"></a>Zadání názvů singulární tabulek

`modelBuilder.Conventions.Remove` Výroky [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda zabraňuje se pluralized názvy tabulek. Pokud jste to neudělali, by se pojmenoval generované tabulky v databázi `Students`, `Courses`, a `Enrollments`. Místo toho budou názvy tabulek `Student`, `Course`, a `Enrollment`. Vývojáři Nesouhlasím o tom, jestli by měl názvy tabulek pluralized nebo ne. Tento kurz používá jednotný tvar, ale důležité je, že můžete vybrat libovolný formulář dáváte přednost zahrnutím nebo vynechání tento řádek kódu.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Nastavit EF inicializovat databázi s testovací data

Entity Framework můžete automaticky vytvořit (nebo vyřadit a znovu vytvořit) databáze za vás při spuštění aplikace. Můžete určit, že to by mělo být provedeno pokaždé, když vaše aplikace spuštěná, nebo jenom v případě modelu je synchronizovaný s existující databází. Můžete je zapsat také `Seed` , Entity Framework automaticky volá po vytvoření databáze, aby bylo možné naplnit ho daty testovací metody.

Výchozí chování je vytvořit databázi jenom v případě, že ho neexistuje (a vyvolání výjimky, pokud došlo ke změně modelu a databáze již existuje). V této části zadáte, že by měl být databázi vyřadit a znovu vytvořit při každé změně modelu. Vyřazení databáze způsobí ztrátu všechna vaše data. Obvykle se jedná OK během vývoje, protože `Seed` metody se spustí, jakmile se znovu vytvoří databázi a znovu vytvoříte testovací data. Ale v produkčním prostředí budete obvykle nechcete ztratit všechna vaše data pokaždé, když je třeba změnit schéma databáze. Později uvidíte, jak zpracovat změny modelu pomocí migrace Code First pro změnu schématu databáze místo vyřadit a znovu vytvořit databázi.

Ve složce DAL, vytvořte nový soubor třídy s názvem *SchoolInitializer.cs* a nahraďte kód šablony se  
Následující kód, což způsobí, že databáze při vytvoření potřebné a načte testovací data do nové databáze.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed` Metoda přijímá jako vstupní parametr objekt kontextu databáze, a kód v metodě používá tento objekt k přidání nové entity do databáze. Pro každý typ entity kód vytvoří kolekci nové entity, se přidají do příslušné `DbSet` vlastnost a uloží změny do databáze. Není nutné volat `SaveChanges` metoda po každé skupiny entit, jako se zde provádí, ale způsobem, který pomáhá při hledání příčiny problému, pokud dojde k výjimce, zatímco kód zapisuje do databáze.

Zjistit Entity Framework pomocí vaší inicializátor třídy, přidejte element, který má `entityFramework` v aplikaci prvku *Web.config* souboru (je v kořenové složce projektu), jak je znázorněno v následujícím příkladu:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type` Určuje kontext plně kvalifikovaný název třídy a sestavení probíhá, a `databaseinitializer type` Určuje plně kvalifikovaný název inicializátor třídy a je v sestavení. (Pokud nechcete, aby EF použití inicializátoru, můžete nastavit atribut na `context` element: `disableDatabaseInitialization="true"`.) Další informace najdete v tématu [Entity Framework – nastavení souboru Config](https://msdn.microsoft.com/data/jj556606).

Jako alternativu k nastavení inicializátoru *Web.config* souboru se to udělat v kódu tak, že přidáte `Database.SetInitializer` příkazu `Application_Start` metoda ve *Global.asax.cs* souboru. Další informace najdete v tématu [Principy inicializátory databáze v Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Aplikace je nyní nastaveno až tak, aby při přístupu k databázi poprvé v daném spuštění  
aplikace, Entity Framework porovnává databáze do modelu (vaše `SchoolContext` a tříd entit). Pokud rozdíl aplikace zahodí a znovu vytvoří databázi.

> [!NOTE]
> Při nasazení aplikace do produkčního prostředí webového serveru, musíte odebrat nebo zakázat kód, který se zahodí a znovu vytvoří databázi. Můžete to udělat v pozdějších kurzech v této sérii.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Nastavit EF k použití databáze SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) je Odlehčená verze SQL serveru Express Database Engine. Snadno nainstalujte a nakonfigurujte, spustí na vyžádání a běží v uživatelském režimu. LocalDB běží v ve speciálním režimu provádění SQL Server Express, která umožňuje pracovat s databází jako *.mdf* soubory. Můžete umístit soubory databáze LocalDB *aplikace\_Data* složce webového projektu, pokud chcete zkopírovat databázi s projektem. Funkce instance uživatele v SQL serveru Express také umožňuje pracovat s *.mdf* soubory, ale uživatelské instance funkce je zastaralá možnost; proto se doporučuje LocalDB pro práci s *.mdf* soubory. V sadě Visual Studio 2012 a novějších verzích je LocalDB nainstalované ve výchozím nastavení se sadou Visual Studio.

Obvykle SQL Server Express se nepoužívá pro produkční webové aplikace. LocalDB zejména se nedoporučuje pro produkční použití s webovou aplikací vzhledem k tomu, že není určená pro práci se službou IIS.

V tomto kurzu budete pracovat s LocalDB. Otevřete aplikaci *Web.config* a přidejte `connectionStrings` předchozí element `appSettings` elementu, jak je znázorněno v následujícím příkladu. (Nezapomeňte aktualizovat *Web.config* soubor v kořenové složce projektu. K dispozici je také *Web.config* soubor *zobrazení* podsložky, která není nutné aktualizovat.)

Pokud používáte Visual Studio 2015, nahraďte text "v11.0" v připojovacím řetězci textem "MSSQLLocalDB", protože výchozí název instance systému SQL Server změnil.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Určuje připojovací řetězec, který jste přidali, Entity Framework použijete databázi LocalDB s názvem *ContosoUniversity1.mdf*. (Databáze ještě neexistuje; EF ji vytvoří.) Pokud byste chtěli databáze, kterou chcete vytvořit v vaše *aplikace\_Data* složky, můžete přidat `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` na připojovací řetězec. Další informace o připojovacích řetězcích najdete v tématu [připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Nemáte ve skutečnosti mít připojovací řetězec *Web.config* souboru. Pokud nezadáte připojovací řetězec, Entity Framework použije výchozí jeden založené na třídě kontextu. Další informace najdete v tématu [Code First pro novou databázi](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Vytvoření Kontroleru studentů a zobrazení

Teď vytvoříte webovou stránku pro zobrazení dat a automaticky spustí proces žádosti o data  
Vytvoření databáze. Zobrazí za přibližně tak, že vytvoříte nový kontroler. Ale předtím, než to uděláte, sestavte projekt a zpřístupnit třídy modelu a kontextu pro generování uživatelského rozhraní řadiče MVC.

1. Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníka řešení**vyberte **přidat**a potom klikněte na tlačítko **novou vygenerovanou položku**.
2. V **přidat vygenerované uživatelské rozhraní** dialogu **kontroler MVC 5 se zobrazeními, používá nástroj Entity Framework**.

     ![Přidat vygenerované uživatelské rozhraní](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. V dialogovém okně Přidat kontroler, proveďte následující výběr a potom klikněte na tlačítko **přidat**:

   - Třída modelu: **Student (ContosoUniversity.Models)**. (Pokud se tato možnost v rozevíracím seznamu nezobrazí, sestavte projekt a zkuste to znovu.)
   - Třída kontextu dat: **SchoolContext (ContosoUniversity.DAL)**.
   - Název kontroleru: **StudentController** (ne StudentsController).
   - Ponechte výchozí hodnoty pro ostatní pole.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Po kliknutí na **přidat**, scaffolder vytvoří soubor StudentController.cs a sadu zobrazení (.cshtml soubory), které pracují s kontrolerem. V budoucnu při vytváření projektů, které využívají Entity Framework můžete taky využít výhod některé další funkce scaffolder: stačí vytvořit svou první třídu modelu, nevytvářejte připojovací řetězec a pak v **přidat kontroler** pole zadejte novou třídu kontextu. Vytvoří scaffolder vaše `DbContext` třídy a připojení řetězec a také kontroler a zobrazení.
4. Visual Studio otevře *Controllers\StudentController.cs* souboru. Uvidíte, že proměnné třídy se vytvořil, který vytvoří instanci objektu kontextu databáze:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index` Metody akce získá seznam studenti z *studenty* načtením sadu entit `Students` vlastnost instance kontextu databáze:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml* zobrazení seznamu v tabulce:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Stiskněte kombinaci kláves CTRL + F5 ke spuštění projektu. (Pokud dojde k chybě "Nejde vytvořit stínovou kopii" zavřete prohlížeč a zkuste to znovu.)

     Klikněte na tlačítko **studenty** kartu pro zobrazení testovacích dat, který `Seed` metoda vložen. V závislosti na tom, jak úzké okno prohlížeče, je, uvidíte odkaz karta studenta nejvyšší adresního řádku nebo budete muset klikněte na tlačítko pravém horním rohu na odkaz.

     ![Tlačítko nabídky](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Student indexová stránka](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Zobrazení databáze

Pokud jste spustili na stránce studenty a pokusu o přístup k databázi aplikace, EF viděli, že neexistuje žádná databáze a proto ho vytvořil, pak ho spustili seed – metoda k naplnění databáze s daty.

Můžete použít buď **Průzkumníka serveru** nebo **Průzkumník objektů systému SQL Server** (SSOX) Chcete-li zobrazit databáze v sadě Visual Studio. Pro účely tohoto kurzu budete používat **Průzkumníka serveru**. (V edici Visual Studio Express starší než 2013, **Průzkumníka serveru** nazývá **Průzkumník databáze**.)

1. Zavřete prohlížeč.
2. V **Průzkumníka serveru**, rozbalte **datová připojení**, rozbalte **školním kontextu (ContosoUniversity)** a potom rozbalte **tabulky** zobrazíte tabulky v nové databázi.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Klikněte pravým tlačítkem na **Student** tabulky a klikněte na tlačítko **zobrazit Data tabulky** zobrazit sloupce, které byly vytvořeny a řádky, které byly vloženy do tabulky.

    ![Tabulka Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Zavřít **Průzkumníka serveru** připojení.

*ContosoUniversity1.mdf* a *.ldf* databázové soubory jsou v `C:\Users\<yourusername>` složky.

Vzhledem k tomu, že používáte `DropCreateDatabaseIfModelChanges` inicializátor, může teď provedete změnu `Student` třídy, spusťte aplikaci znovu spustit a databáze bude automaticky znovu vytvořit tak, aby odpovídaly změny. Například, pokud chcete přidat `EmailAddress` vlastnost `Student` třídy, znovu spusťte stránce studenty a pak pohled na tabulku znovu, zobrazí se nový `EmailAddress` sloupce.

## <a name="conventions"></a>Konvence

Množství kódu, které jste měli pro zápis v pořadí pro Entity Framework umožnit vytvoření kompletní databáze je minimální, protože se používá *konvence*, nebo předpokladů, které díky rozhraní Entity Framework. Některé z nich už bylo uvedeno nebo byly použity bez vašeho vědomí:

- Pluralized formy názvy tříd entit se používají jako názvy tabulek.
- Názvy vlastností entity se používají pro názvy sloupců.
- Vlastnosti entity, které jsou pojmenovány `ID` nebo *classname* `ID` jsou rozpoznány jako vlastnosti primárního klíče.
- Vlastnost je interpretován jako vlastnost cizího klíče, pokud je název *&lt;název navigační vlastnosti&gt;&lt;vlastnost primárního klíče název&gt;* (například `StudentID` pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`). Vlastnosti cizího klíče může také být pojmenován stejně jednoduše &lt;vlastnost primárního klíče název&gt; (například `EnrollmentID` od `Enrollment` je primární klíč entity `EnrollmentID`).

Už víte, že konvence lze přepsat. Například jste zadali, že by neměla být pluralized názvy tabulek a později uvidíte, jak lze explicitně označit vlastnost jako vlastnost cizího klíče. Získáte další informace o vytváření a jak je v přepsat [vytváření více komplexní datový Model](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) později v této sérii kurzů. Další informace o konvencích najdete v tématu [první konvence kódu](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Souhrn

Právě jste vytvořili jednoduchou aplikaci, která se používá k ukládání a zobrazení dat Entity Framework a SQL Server Express LocalDB. V následujícím kurzu dozvíte jak provést základní CRUD (vytváření, čtení, aktualizace nebo odstranění) operace.

Jak vám v tomto kurzu líbilo a co můžeme zlepšit nám prosím zpětnou vazbu. Můžete také požádat o nový témat na [Show Me jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework lze nalézt v [přístup k datům ASP.NET – doporučené zdroje informací](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
