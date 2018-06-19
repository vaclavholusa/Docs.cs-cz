---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Načítání a zobrazení dat s model vazby a webových formulářů | Microsoft Docs
author: tfitzmac
description: Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889092"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Načítání a zobrazování dat pomocí vazby modelu a webové formuláře
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource). Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.
> 
> Vzor vazby modelu funguje s technologií pro přístup k žádné data. V tomto kurzu budete používat rozhraní Entity Framework, ale můžete použít technologie přístup data, která je pro vás nejvíce dobře známé. Z ovládacího prvku serveru vázané na data, jako je například ovládacího prvku GridView ListView, DetailsView nebo FormView zadejte názvy metod, které chcete použít pro výběr, aktualizaci, odstranění a vytvoření data. V tomto kurzu bude zadejte hodnotu pro metody SelectMethod.
> 
> V rámci této metody poskytují logiku pro načítání dat. V dalším kurzu nastavíte hodnoty pro metodu UpdateMethod, metoda DeleteMethod a metodu InsertMethod.
> 
> Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB. Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013. Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.
> 
> V tomto kurzu spusťte aplikaci v sadě Visual Studio. Můžete provést také aplikace dostupné přes Internet pomocí nasazení do hostujícího zprostředkovatele. Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webových serverů ve  
>  [Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Informace o tom, jak nasadit webový projekt sady Visual Studio do Azure App Service Web Apps naleznete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) řady. Tento kurz také ukazuje, jak nasadit databázi SQL serveru do Azure SQL Database pomocí migrace Entity Framework Code First.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Microsoft Visual Studio 2013 nebo Microsoft Visual Studio Express 2013 pro Web
>   
> 
> V tomto kurzu funguje taky s Visual Studio 2012, ale bude určité rozdíly v šabloně uživatelské rozhraní a projekt.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu budete:

1. Vytvoření datových objektů, které odráží vysoké školy s studenty zaregistrované v rámci přípravy
2. Vytvoření databázových tabulek z objektů
3. Naplnění databáze testovací data
4. Zobrazení dat v webového formuláře

## <a name="set-up-project"></a>Nastavení projektu

V sadě Visual Studio 2013, vytvořte novou **webové aplikace ASP.NET** názvem **ContosoUniversityModelBinding**.

![Vytvoření projektu](retrieving-data/_static/image2.png)

Vyberte šablonu, webových formulářů a nechte výchozí možnosti. Klikněte na tlačítko OK nastavení projektu.

![Vyberte webové formuláře](retrieving-data/_static/image3.png)

Bude nutné mít několik malé změny přizpůsobení vzhledu webu. Otevřete **Site.Master** soubor a změňte název zahrnout Contoso univerzity místo Moje aplikace technologie ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Změňte text záhlaví z **název aplikace** k **Contoso univerzity**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Také v Site.Master, můžete změňte navigační odkazy, které se zobrazují v hlavičce tak, aby odrážela stránek, které jsou relevantní pro tuto lokalitu. Nebudete potřebovat buď **o** stránky nebo **kontaktujte** stránky tak může tyto odkazy odebrat. Místo toho je nutné odkaz na stránku s názvem **studenty**. Tato stránka ještě nebyl vytvořen.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Uložte a zavřete Site.Master.

Teď vytvoříte webového formuláře pro zobrazení dat student. Klikněte pravým tlačítkem na projekt, a **přidat** **novou položku**. Vyberte **webové formuláře se stránkou předlohy** šablony a pojmenujte ji **Students.aspx**.

![Vytvoření stránky](retrieving-data/_static/image5.png)

Vyberte **Site.Master** jako stránky předlohy pro nový webový formulář.

## <a name="create-the-data-models-and-database"></a>Vytvoření datové modely a databáze

Migrace Code First použije k vytvoření objektů a odpovídající databázových tabulek. Tyto tabulky se uloží informace o studenty a jejich kurzy.

Ve složce modely, přidejte novou třídu s názvem **UniversityModels.cs**.

![Vytvoření třídy modelu](retrieving-data/_static/image7.png)

V tomto souboru Definujte třídy SchoolContext, studenty, registraci a během takto:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

Třída SchoolContext je odvozena od DbContext, která spravuje připojení k databázi a změny v datech.

Ve třídě studenty, Všimněte si, atributy, které se aplikovaly **FirstName**, **LastName**, a **roku** vlastnosti. Tyto atributy se použije pro ověření dat v tomto kurzu. Můžete zjednodušit kód pro tento ukázkový projekt, byly s atributy ověření dat označeny pouze tyto vlastnosti. V skutečné projektu platit na všechny vlastnosti, které je třeba ověřená data, například vlastnosti ve třídách registraci a během atributů ověření.

Uložte UniversityModels.cs.

Nástroje pro migrace Code First budete používat k nastavení databáze, založený na těchto třídách.

V **Konzola správce balíčků**, spusťte příkaz:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Po úspěšném dokončení příkazu obdržíte zprávu s oznámením povoleným migrace

![Povolit migrace](retrieving-data/_static/image8.png)

Všimněte si, že nový soubor s názvem **Configuration.cs** byla vytvořena. V sadě Visual Studio je tento soubor otevřít automaticky po jejím vytvoření. Obsahuje třídu konfigurace **počáteční hodnoty** metodu, která umožňuje k předběžnému naplnění tabulky databáze s testovacích datech.

Přidejte následující kód do metody počáteční hodnoty. Budete muset přidat **pomocí** údajů **ContosoUniversityModelBinding.Models** oboru názvů.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Uložte Configuration.cs.

V konzole Správce balíčků, spusťte příkaz `add-migration initial`.

Spusťte příkaz `update-database`.

Pokud se zobrazí výjimku při spuštění tohoto příkazu, je možné, že mají různé hodnoty pro StudentID a CourseID z hodnot v metodě počáteční hodnoty. Otevřete tyto tabulky v databázi a vyhledat existující hodnoty pro StudentID a CourseID. Přidejte tyto hodnoty v kódu pro synchronizace replik indexů v tabulce registrace.

Soubor databáze byl přidán, ale je aktuálně skrytá v projektu. Klikněte na tlačítko **zobrazit všechny soubory** zobrazit soubor.

![Zobrazit všechny soubory](retrieving-data/_static/image10.png)

Všimněte si souboru .mdf se teď zobrazí v aplikaci\_složku Data.

![soubor databáze](retrieving-data/_static/image12.png)

Poklikejte na soubor MDF a otevřete Průzkumníka serveru. V tabulkách teď existují a jsou naplněný daty.

![tabulky databáze](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Zobrazit data z studenty a související tabulky

S daty v databázi nyní jste připraveni k načtení dat a jejich zobrazení na webové stránce. Budete používat **GridView** ovládací prvek pro zobrazení dat ve sloupci a řádky.

Otevřete Students.aspx a vyhledejte **MainContent** zástupný symbol. V rámci zástupného symbolu, přidejte **GridView** ovládací prvek, který obsahuje následující kód.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Existuje několik důležité koncepty v Všimněte tímto kódem značek. První, Všimněte si, že je zadána hodnota pro **metody SelectMethod** vlastnost v elementu GridView. Tato hodnota určuje název metody, která se používá k načítání dat pro GridView. Tato metoda vytvoří v dalším kroku. Druhý, Všimněte si, že **ItemType** je nastavena na třídě studenty, kterou jste vytvořili dříve. Nastavením této hodnoty lze odkazovat k vlastnostem této třídy v kódu značek. Například třída Student obsahuje kolekci s názvem registrace. Můžete použít **Item.Enrollments** načíst tuto kolekci a pak použijte syntaxi LINQ načtení součet zaregistrovaná kredity pro každý student.

V souboru kódu na pozadí, budete muset přidat metodu, která je určená pro **metody SelectMethod** hodnotu. Otevřete **Students.aspx.cs**a přidejte **pomocí** pro příkazy **ContosoUniversityModelBinding.Models** a **System.Data.Entity**obory názvů.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Pak přidejte následující metodu. Všimněte si, že název tato metoda odpovídá názvu, který jste zadali pro metody SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**Zahrnout** klauzule zvyšuje výkon tohoto dotazu, ale není nezbytné pro dotaz pro práci. Bez klauzuli zahrnout by načíst data pomocí opožděného načítání, který zahrnuje odeslání samostatné dotaz do databáze pokaždé, když související s načíst data. Tím, že poskytuje klauzuli zahrnout, data se načítají pomocí přes načítání, což znamená, všechna související data se načítají prostřednictvím jednoho dotazu databáze. V případech, kde je většina souvisejících dat nesmí být může být použit, přes načítání méně efektivní, protože načítat další data. Nicméně v takovém případě přes načítání poskytuje nejlepší výkon vzhledem k tomu, že se pro každý záznam zobrazí související data.

Pro další informace o výkonu zvážení při načítání související data, najdete v části s názvem **Lazy Eager a explicitní načítání souvisejících dat** v [čtení související Data pomocí rozhraní Entity Framework v ASP Aplikace .NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tématu.

Ve výchozím nastavení data jsou seřazená podle hodnot vlastnosti označen jako klíč. Můžete přidat klauzuli OrderBy chcete zadat jinou hodnotu pro řazení. V tomto příkladu se používá výchozí vlastnost StudentID pro řazení. V [řazení, stránkování a filtrování dat](sorting-paging-and-filtering-data.md) tématu, vám umožní uživateli vybrat sloupec pro řazení.

Spuštění webové aplikace a přejděte na stránku studenty. Na stránce studenti, kteří se zobrazí následující informace student.

![Zobrazit data](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Automatické generování metody vazby modelu

Při práci přes tento kurz řady, můžete jednoduše zkopírovat kód z tohoto kurzu do projektu. Jednou z nevýhod tohoto přístupu je však, nemusí se dozvěděli o funkce poskytované sadě Visual Studio k automatickému generování kódu pro metody vazby modelu. Při práci na vašich vlastních projektů, automatické generování kódu můžete ušetřit čas a získat představu o tom, jak implementovat operaci. Tato část popisuje funkci generování automatické kódu. Tato část je pouze informativní a neobsahuje žádný kód, který je nutné implementovat ve vašem projektu.

Při nastavení hodnoty pro **metody SelectMethod**, **metody UpdateMethod**, **metodu InsertMethod**, nebo **metody DeleteMethod** vlastností v kódu značek můžete vybrat **vytvoření nové metody** možnost.

![Vytvoření nové metody](retrieving-data/_static/image18.png)

Visual Studio nejen vytvoří metodu v kódu s správný podpis, ale také generuje kód implementace a pomůže vám při provádění této operace. Pokud je nejdřív nastavit **ItemType** vlastnost před použitím funkci generování automatické kód generovaný kód použije tento typ pro operace. Například při nastavení této vlastnosti metody UpdateMethod, následující kód se automaticky vygeneroval:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Výše uvedený kód znovu, není nutné přidat do projektu. V dalším kurzu budete implementovat metody pro aktualizaci, odstranění a přidání nová data.

## <a name="conclusion"></a>Závěr

V tomto kurzu jste vytvořili třídy modelu dat a generuje databázi z těchto tříd. Tabulky databáze, které se naplní se testovacích datech. Použít vazby modelu k načtení dat z databáze a pak se zobrazují data v GridView.

V dalším [kurzu](updating-deleting-and-creating-data.md) této série povolíte aktualizaci, odstranění a vytvoření data.

> [!div class="step-by-step"]
> [Next](updating-deleting-and-creating-data.md)
