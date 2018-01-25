---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "Začínáme s Entity Framework 6 Code First pomocí MVC 5 | Microsoft Docs"
author: tdykstra
description: "Je k dispozici novější verze Tato řada kurz: Začínáme s ASP.NET Core a Entity Framework Core pomocí sady Visual Studio 2015. Contoso Universi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 46f53279e2e6daa4266c06feb4ba544e14b68a03
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Začínáme s Entity Framework 6 Code First pomocí MVC 5
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Je k dispozici novější verze tohoto kurzu řad: [Začínáme s ASP.NET Core a Entity Framework Core pomocí sady Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 a Visual Studio 2013. Tento kurz používá Code First pracovního postupu. Informace o tom, jak vybrat jednu z Code First, Database First nebo Model First najdete v tématu [Entity Framework vývoj pracovních](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> Vzorová aplikace je web pro fiktivní vysoké školy Contoso. Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem. Tento kurz řady vysvětluje, jak vytvořit Contoso univerzity ukázkovou aplikaci. Můžete [stáhnout hotová aplikace](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Visual Basic verze přeložen Karel Brind je k dispozici: [MVC 5 s EF 6 v jazyce Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) na webu Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Rozhraní Entity Framework 6 (balíček NuGet EntityFramework 6.1.0)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (volitelné)
>   
> 
> Tento kurz by taky měly fungovat s [Visual Studio 2013 Express pro Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) nebo Visual Studio 2012. [VS 2012 verzi sady Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) je vyžadována pro nasazení systému Windows Azure pomocí sady Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Kurz verze
> 
> Předchozí verze tohoto kurzu, najdete v části [EF 4.1 nebo elektronická kniha MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) a [Začínáme s EF 5 pomocí MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx), [Entity Framework a technologie LINQ to Entities fórum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), nebo [ StackOverflow.com](http://stackoverflow.com/).
> 
> Pokud narazíte na potíže, které nelze vyřešit, můžete najít řešení problému obecně tak, že porovnáte kódu pro dokončené projekt, který si můžete stáhnout. Pro některé běžné chyby a jak je vyřešit, najdete v části [běžné chyby a řešení či alternativní řešení pro ně.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Contoso univerzity webové aplikace

Aplikace, kterou jste budete vytvářet v těchto kurzech je jednoduchý univerzity webovou stránku.

Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace. Zde najdete několik z obrazovky, které vytvoříte.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Upravit studenty](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Styl uživatelského rozhraní tohoto webu byla choval blízko co je generován integrované šablony tak, aby tento kurz můžete zaměřit především na tom, jak používat rozhraní Entity Framework.

## <a name="prerequisites"></a>Požadavky

V tématu **verze softwaru** v horní části stránky. Rozhraní Entity Framework 6 není předpokladem, protože nainstalujte balíček EF NuGet jako součást tohoto kurzu.

## <a name="create-an-mvc-web-application"></a>Vytvořit webovou aplikaci MVC

Otevřete Visual Studio a vytvořte nový projekt C# webovou s názvem "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

V **nový projekt ASP.NET** dialogovém okně vyberte pole **MVC** šablony.

Pokud **hostovat v cloudu** zaškrtávací políčko **Microsoft Azure** je vybraná část, vymazat.

Klikněte na tlačítko **změnit ověřování**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

V **změna ověřování** dialogové okno, vyberte **bez ověřování**a potom klikněte na **OK**. V tomto kurzu se by uživatelé museli přihlásit ani nebudete omezení přístup podle toho, který je přihlášen.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Zpět v dialogovém okně Nový projekt ASP.NET, klikněte na tlačítko **OK** a vytvořte tak projekt.

## <a name="set-up-the-site-style"></a>Nastavení stylu lokality

Pár jednoduchými změnami nastaví lokality nabídky, rozložení a domovské stránky.

Otevřete *Views\Shared\\_Layout.cshtml*a proveďte následující změny:

- Změňte každý výskyt "Moje aplikace technologie ASP.NET" a "Název aplikace" na "Univerzity Contoso".
- Přidání položek nabídky pro studenty, kurzy, vyučující a oddělení a kontaktujte položku odstranit.

Změny se zvýrazněnou.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

V *Views\Home\Index.cshtml*, nahraďte obsah souboru následující kód, který nahradí text o ASP.NET a MVC o této aplikaci:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Stiskněte kombinaci kláves CTRL + F5 spusťte web. Zobrazí domovská stránka s v hlavní nabídce.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Nainstalujte rozhraní Entity Framework 6

Z **nástroje** nabídce klikněte na tlačítko **Správce balíčků NuGet** a pak klikněte na **Konzola správce balíčků**.

V **Konzola správce balíčků** okno, zadejte následující příkaz:

`Install-Package EntityFramework`

![EF nainstalován](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Obrázek ukazuje 6.0.0 instalován, ale NuGet nainstaluje nejnovější verzi rozhraní Entity Framework (s výjimkou předprodejní verze), která od poslední aktualizace tohoto kurzu je 6.1.1.

Tento krok je jedním z několika kroků v tomto kurzu se můžete udělat ručně, že což může mít bylo provedeno automaticky funkce generování uživatelského rozhraní ASP.NET MVC. Vaše změny je ručně tak, aby se zobrazí na kroky potřebné k použití rozhraní Entity Framework. Generování uživatelského rozhraní budete používat později vytvoření MVC jsou řadič MVC a zobrazení. Alternativou je umožníte generování uživatelského rozhraní automaticky nainstalovat balíček EF NuGet, vytvoření třídy kontextu databáze a vytvořit připojovací řetězec. Když jste připraveni provést tímto způsobem, je všechny, které musíte udělat tyto kroky přeskočit a vygenerovat vašeho řadiče MVC po vytvoření třídy entity.

## <a name="create-the-data-model"></a>Vytvoření datového modelu

Dále vytvoříte tříd entit pro danou aplikaci univerzity Contoso. Budete začínat následující tři entity:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Je vztah jeden mnoho mezi `Student` a `Enrollment` entity, a je mezi vztah jeden mnoho `Course` a `Enrollment` entity. Jinými slovy student může být zaregistrované v libovolný počet kurzy a kurzu může mít libovolný počet studenti, kteří jsou v ní zaregistrované.

V následujících částech vytvoříte třídu pro každé z nich o těchto entitách.

> [!NOTE]
> Pokud se pokusíte kompilace projektu, dokud nedokončíte všechny tyto entity třídy vytváření, získáte chyby kompilátoru.


### <a name="the-student-entity"></a>Student Entity

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

V *modely* složky, vytvořte soubor třídy s názvem *Student.cs* a nahraďte kód šablony s následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Vlastnost bude sloupec primárního klíče tabulky databáze, která odpovídá této třídy. Ve výchozím nastavení, rozhraní Entity Framework interpretuje vlastnost s názvem `ID` nebo *classname* `ID` jako primární klíč.

`Enrollments` Vlastnost je *navigační vlastnost*. Navigační vlastnosti podržte dalšími subjekty, které se vztahují k této entity. V takovém případě `Enrollments` vlastnost `Student` entity bude obsahovat všechny `Enrollment` entit, které se vztahují k které `Student` entity. Jinými slovy pokud dané `Student` řádků v databázi má dva související `Enrollment` řádků (hodnota řádky, které obsahují tento Studentova primární klíč v jejich `StudentID` sloupec cizího klíče), že `Student` entity `Enrollments` navigační vlastnost bude obsahovat tyto dvě `Enrollment` entity.

Navigační vlastnosti jsou obvykle definovány jako `virtual` tak, aby mohou využít určitých funkcí rozhraní Entity Framework, jako *opožděného načítání*. (Vysvětlení opožděného načítání naleznete dále v [související Data čtení](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurz později z této série.)

Pokud vlastnost navigace mohou být uloženy více entit (jako relace m: n nebo na více), jeho typ musí být seznam, ve kterém položky možné ji přidat, odstranit nebo aktualizovat, například `ICollection`.

### <a name="the-enrollment-entity"></a>Registrace Entity

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

V *modely* složku vytvořit *Enrollment.cs* a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Primární klíč, bude mít vlastnost; používá tuto entitu *classname* `ID` vzor místo `ID` samostatně jako jste viděli v `Student` entity. By normálně zvolte jeden vzor a použít ho v rámci datového modelu. Zde variaci znázorňuje, které můžete použít buď vzor. Novější kurzu, uvidíte jak pomocí `ID` bez `classname` usnadňuje implementaci dědičnosti v datovém modelu.

`Grade` Vlastnost je [výčtu](https://msdn.microsoft.com/data/hh859576.aspx). Otazník po `Grade` deklaraci typu znamená, že `Grade` vlastnost je [s možnou hodnotou Null](https://msdn.microsoft.com/library/2cf62fcy.aspx). Třída, která má hodnotu null se liší od nulové úrovni – hodnota null znamená úrovni není známý nebo ještě nebyly přiřazeny.

`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Student`. `Enrollment` Entita je spojen s jednou `Student` entit, tak vlastnost mohou obsahovat pouze jeden `Student` entity (na rozdíl od `Student.Enrollments` navigační vlastnost jste viděli dříve, která může pojmout více `Enrollment` entity).

`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Course`. `Enrollment` Entita je spojen s jednou `Course` entity.

Rozhraní Entity Framework interpretuje vlastnost jako vlastnost cizího klíče, pokud je název  *&lt;název vlastnosti navigace&gt;&lt;vlastnost primárního klíče název&gt;*  (například `StudentID`pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`). Vlastnosti cizího klíče můžete také se stejným názvem, jednoduše  *&lt;vlastnost primárního klíče název&gt;*  (například `CourseID` vzhledem k tomu `Course` je primární klíč entity `CourseID`).

### <a name="the-course-entity"></a>Během Entity

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

V *modely* složku vytvořit *Course.cs*, nahraďte kód šablony s následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` Je navigační vlastnost. A `Course` entity může souviset s libovolný počet `Enrollment` entity.

Budete říkáme, informace o [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) atribut novější kurzu této série. V zásadě platí tento atribut slouží k zadání primární klíč pro během, místo aby databázi jeho vygenerování.

## <a name="create-the-database-context"></a>Vytvoření kontextu databáze

Hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model je *kontext databáze* třídy. Vytvořit této třídy odvozené z [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) třídy. V kódu zadáte entit, které jsou zahrnuty v datovém modelu. Můžete také upravit chování určité Entity Framework. V tomto projektu je třída s názvem `SchoolContext`.

Chcete-li vytvořit složku v ContosoUniversity projektu, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a klikněte na tlačítko **přidat**a potom klikněte na **novou složku**. Název nové složky *DAL* (pro Data Access Layer). V této složce vytvořte soubor novou třídu s názvem *SchoolContext.cs*a kód šablony nahraďte následujícím kódem:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Zadání sad entit

Tento kód vytvoří [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) vlastnosti pro každou sadu entit. V terminologii rozhraní Entity Framework *sady entit* obvykle odpovídá do databázové tabulky a *entity* odpovídá na řádek v tabulce.

> [!NOTE] 
> 
> Vám může mít tento parametr vynechán `DbSet<Enrollment>` a `DbSet<Course>` příkazy a bude fungovat stejně. Rozhraní Entity Framework by mělo zahrnovat je implicitně protože `Student` odkazy na entity `Enrollment` entity a `Enrollment` odkazy na entity `Course` entity.


### <a name="specifying-the-connection-string"></a>Určení připojovací řetězec

Název připojovacího řetězce (který přidáte do souboru Web.config později) je předána do konstruktoru.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Také můžete předat v připojovacím řetězci samotné místo názvu jednoho, který je uložený v souboru Web.config. Další informace o možnosti pro zadání databáze, které se mají použít, najdete v části [Entity Framework - připojení a modely](https://msdn.microsoft.com/data/jj592674).

Pokud nezadáte připojovací řetězec nebo název jednoho explicitně, rozhraní Entity Framework předpokládá, že název připojovacího řetězce je stejný jako název třídy. Výchozí název připojovacího řetězce v tomto příkladu by pak bylo `SchoolContext`, stejná jako co určujete explicitně.

### <a name="specifying-singular-table-names"></a>Určení názvů singulární tabulek

`modelBuilder.Conventions.Remove` Příkaz v [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) metoda zabrání se pluralized názvy tabulek. Pokud nebylo to uděláte, by generovaného tabulky v databázi s názvem `Students`, `Courses`, a `Enrollments`. Místo toho budou názvy tabulek `Student`, `Course`, a `Enrollment`. Vývojáři Nesouhlasím o tom, jestli by měl názvy tabulek pluralized nebo ne. Tento kurz používá jednotném čísle, ale důležité je, že můžete vybrat libovolného tvaru dáváte přednost zahrnutím nebo vynechání tento řádek kódu.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Nastavit EF k chybě při inicializaci databáze s testovací data

Rozhraní Entity Framework můžete automaticky vytvořit (nebo vyřadit a znovu vytvořit) databáze pro vás při spuštění aplikace. Můžete určit, že to by mělo být provedeno pokaždé, když vaše aplikace běží, nebo jenom v případě modelu je mimo rozsah synchronizace s existující databáze. Je také možné zapsat `Seed` metoda, aby rozhraní Entity Framework automaticky volá po vytvoření databáze k naplnění testovacích datech.

Výchozí chování je vytvořit databázi pouze v případě, že ho neexistuje (a způsobí výjimku, pokud model byl změněn a databáze již existuje). V této části specifikujete, že by měl být databázi vyřadit a znovu vytvořit při každé změně modelu. Vyřazení databáze způsobí ztrátu všechna vaše data. Obecně se to OK během vývoje, protože `Seed` metoda se spustí, když databáze se znovu vytvoří a bude znovu vytvořit testovací data. Ale v produkčním prostředí obecně nechcete ztratit všechna vaše data pokaždé, když je třeba změnit schéma databáze. Později uvidíte, jak zpracovat změny modelu s použitím migrace Code First, chcete-li změnit schéma databáze místo vyřadit a znovu vytvořit databázi.

Ve složce DAL, vytvořte soubor novou třídu s názvem *SchoolInitializer.cs* a nahraďte kód šablony pomocí  
Následující kód, který způsobí, že databáze do vytvořen, pokud nejsou potřebné a načte testovací data do nové databáze.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed` Metoda přebírá objekt kontextu databáze jako vstupní parametr a používá kód v metodě  
Tento objekt přidat nové entity do databáze. Ke každému typu entity kód vytvoří kolekci nové  
 entity, se přidají do příslušné `DbSet` vlastnost a uloží změny do databáze. Není  
potřebné k volání `SaveChanges` po každé skupiny entit, jako je na jednom místě, ale to, která pomáhá – metoda  
Pokud dojde k výjimce při kód je zápis do databáze se nalézt zdroj problém.

Rozhraní Entity Framework používat vlastní třídy inicializátoru říct, přidat element do `entityFramework` element v aplikaci *Web.config* souboru (ten, v kořenové složce projektu), jak je znázorněno v následujícím příkladu:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type` Určuje kontextu plně kvalifikovaný název třídy a sestavení je v, a `databaseinitializer type` Určuje plně kvalifikovaný název třídy inicializátoru a je v sestavení. (Pokud nechcete, aby EF používat inicializátoru, můžete nastavit atribut na `context` element: `disableDatabaseInitialization="true"`.) Další informace najdete v tématu [Entity Framework – nastavení souboru Config](https://msdn.microsoft.com/data/jj556606).

Jako alternativu k nastavení inicializátoru v *Web.config* soubor je to udělat v kódu přidáním `Database.SetInitializer` příkaz, který má `Application_Start` metoda v *Global.asax.cs* souboru. Další informace najdete v tématu [Principy inicializátory databáze v Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Aplikace je teď nastavená až tak, aby při přístupu k databázi poprvé v dané spuštění  
aplikace rozhraní Entity Framework porovná databázi do modelu (vaše `SchoolContext` a tříd entit,). Pokud existuje rozdíl, aplikace zahodí a znovu vytvoří databázi.

> [!NOTE]
> Při nasazení aplikace na produkční webový server, musíte odebrat nebo zakázat kód, který zahodí a znovu vytvoří databázi. Můžete to udělat v novější kurzu této série.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Nastavení EF k použití databáze SQL serveru Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) je Odlehčená verze SQL Server Express Database Engine. Je snadno nainstalovat a nakonfigurovat, spustí na vyžádání a běží v uživatelském režimu. LocalDB běží v režimu speciální spuštění systému SQL Server Express, který umožňuje pracovat s databázemi jako *.mdf* soubory. Můžete umístit soubory databáze LocalDB *aplikace\_Data* složky webový projekt, pokud chcete umožnit kopírování databáze se na projektu. Instance funkce uživatel v systému SQL Server Express také umožňuje pracovat s *.mdf* soubory, ale funkce instance uživatel je zastaralý, tedy LocalDB doporučuje se pro práci s *.mdf* soubory. V sadě Visual Studio 2012 a novějších verzích je ve výchozím nastavení pomocí sady Visual Studio nainstalována LocalDB.

Obvykle SQL Server Express se nepoužije pro provozní webové aplikace. LocalDB zejména se nedoporučuje pro produkční použití s webovou aplikací protože není určeno k práci se službou IIS.

V tomto kurzu budete pracovat s LocalDB. Otevřete aplikaci *Web.config* souboru a přidejte `connectionStrings` element předchozí `appSettings` elementu, jak je znázorněno v následujícím příkladu. (Zajistěte, aby je aktualizovat *Web.config* soubor v kořenové složce projektu. K dispozici je také *Web.config* soubor *zobrazení* podsložky, která není nutné aktualizovat.)

Pokud používáte Visual Studio 2015, nahraďte text "v11.0" v připojovacím řetězci textem "MSSQLLocalDB", protože došlo ke změně výchozí název instance systému SQL Server.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Připojovací řetězec, který jste přidali Určuje, že rozhraní Entity Framework bude používat LocalDB databáze s názvem *ContosoUniversity1.mdf*. (Databáze ještě neexistuje; EF ji vytvoří.) Pokud byste chtěli vytvořit v databázi vaší *aplikace\_Data* složky, můžete přidat `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` připojovací řetězec. Další informace o připojovacích řetězcích najdete v tématu [připojovací řetězce SQL serveru pro webové aplikace ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Získat připojovací řetězec v nemáte ve skutečnosti *Web.config* souboru. Pokud nezadáte připojovací řetězec, rozhraní Entity Framework použije výchozí jeden založené na třídě kontextu. Další informace najdete v tématu [Code First pro novou databázi](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Probíhá vytváření řadič Student a zobrazení

Teď vytvoříte webovou stránku pro zobrazení dat a automaticky spustí proces požaduje data  
Vytvoření databáze. Budete zahájíte vytváření nového řadiče. Ale předtím, než můžete udělat, sestavte projekt, aby byly k dispozici generování uživatelského rozhraní řadiče MVC třídy modelu a kontextu.

1. Klikněte pravým tlačítkem myši **řadiče** složky v **Průzkumníku řešení**, vyberte **přidat**a potom klikněte na **novou vygenerovanou položku**.
- V **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework**.

    ![Přidat vygenerované uživatelské rozhraní](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
- V dialogovém okně Přidat kontroler, vyberte následující možnosti a pak klikněte na **přidat**:

    - Třída modelu: **Student (ContosoUniversity.Models)**. (Pokud tato možnost v rozevíracím seznamu nezobrazí, sestavte projekt a zkuste to znovu.)
    - Třída kontextu dat: **SchoolContext (ContosoUniversity.DAL)**.
    - Název řadiče: **StudentController** (ne StudentsController).
    - Ponechte výchozí hodnoty pro ostatní pole.

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Když kliknete na tlačítko **přidat**, scaffolder vytvoří soubor StudentController.cs a sadu zobrazení (.cshtml soubory), které pracují s řadičem. V budoucnu při vytváření projektů, které používají rozhraní Entity Framework můžete také využít výhod některé další funkce scaffolder: právě vytvoření vaší první třídy modelu, nevytvářejte připojovací řetězec a potom v **přidat kontroler** pole zadejte novou třídu kontextu. Vytvoří scaffolder vaše `DbContext` třídy a připojení řetězce a také řadiče a zobrazení.
- Otevře se Visual Studio *Controllers\StudentController.cs* souboru. Uvidíte, že byl vytvořen proměnné třídy, který vytvoří instanci objektu kontextu databáze:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    `Index` Metoda akce získá seznam studenty z *studenty* načtením sadu entit `Students` vlastností kontextu instance databáze:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    *Student\Index.cshtml* zobrazení zobrazí tento seznam v tabulce:

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
- Stisknutím kombinace kláves CTRL + F5 a spusťte projekt. (Pokud dojde k chybě "Nemůže vytvořit stínovou kopii", zavřete prohlížeč a zkuste to znovu.)

    Klikněte na tlačítko **studenty** karty zobrazíte testovací data, `Seed` metoda vložit. V závislosti na tom, jak úzké okno prohlížeče je, uvidíte odkaz Student karty na panelu Adresa horní nebo budete muset klikněte na pravém horním rohu na odkaz zobrazíte.

    ![Tlačítko nabídky](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    ![Student indexovou stránku](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Zobrazení databáze

Pokud jste spustili studenty stránky a aplikace se pokusila přistoupit k databázi, EF viděli, že se žádná databáze, a proto ho vytvořil, potom byl spuštěn metodu počáteční hodnoty k naplnění databáze s daty.

Můžete použít buď **Průzkumníka serveru** nebo **Průzkumník objektů systému SQL Server** (SSOX) Chcete-li zobrazit databázi v sadě Visual Studio. Pro účely tohoto kurzu budete používat **Průzkumníka serveru**. (V Visual Studio Express edicích starší než 2013, **Průzkumníka serveru** nazývá **Průzkumník databáze**.)

1. Zavřete prohlížeč.
2. V **Průzkumníka serveru**, rozbalte položku **připojení dat**, rozbalte položku **školní kontextu (ContosoUniversity)**a potom rozbalte **tabulky** zobrazíte tabulky v nové databáze.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Klikněte pravým tlačítkem myši **Student** tabulky a klikněte na tlačítko **zobrazit Data tabulky** sloupce, které byly vytvořeny a řádky, které byly vloženy do tabulky.

    ![Tabulka Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Zavřít **Průzkumníka serveru** připojení.

*ContosoUniversity1.mdf* a *.ldf* databázové soubory jsou v `C:\Users\<yourusername>` složky.

Vzhledem k tomu, že používáte `DropCreateDatabaseIfModelChanges` inicializátoru, může nyní provedete změnu `Student` třídy, spusťte aplikaci znovu a databáze bude automaticky znovu vytvořit tak, aby odpovídaly vaší změn. Například, pokud přidáte `EmailAddress` vlastnost, která má `Student` třídy, znovu spusťte stránce studenty a pak pohled na tabulky znovu, zobrazí se nové `EmailAddress` sloupce.

## <a name="conventions"></a>Konvence

Kvůli použití je minimální množství kódu, které jste měli k zápisu, aby rozhraní Entity Framework, abyste mohli vytvořit databázi dokončení pro vás *konvence*, nebo předpoklady, které umožňuje Entity Framework. Některé z nich již bylo uvedeno nebo byly použity bez vašeho vědomí:

- Pluralized formy názvy tříd entity se používají jako názvy tabulek.
- Názvy vlastností entity se používají pro názvy sloupců.
- Vlastnosti entity, které jsou s názvem `ID` nebo *classname* `ID` jsou rozpoznán jako vlastnosti primárního klíče.
- Vlastnost interpretována jako vlastností cizího klíče, pokud je název  *&lt;název vlastnosti navigace&gt;&lt;vlastnost primárního klíče název&gt;*  (například `StudentID` pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`). Vlastnosti cizího klíče můžete také se stejným názvem, jednoduše &lt;vlastnost primárního klíče název&gt; (například `EnrollmentID` vzhledem k tomu `Enrollment` je primární klíč entity `EnrollmentID`).

Seznámili jste se, že je možné přepsat konvence. Například jste zadali, že by neměl být pluralized názvy tabulek, a budete později jak explicitně označit vlastnost jako vlastnost cizího klíče. Budete Další informace o konvence a jak přepsat je do [vytváření další komplexní Model dat](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) kurz později z této série. Další informace o konvencích najdete v tématu [první pravidla týkající se kódu](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Souhrn

Nyní jste vytvořili jednoduchou aplikaci, která se používá k uložení a zobrazení data Entity Framework a SQL Server Express LocalDB. V následujícím kurzu se dozvíte jak provádět základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) operace.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
