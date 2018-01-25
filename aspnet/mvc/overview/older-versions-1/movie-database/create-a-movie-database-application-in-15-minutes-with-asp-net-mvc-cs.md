---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: "Vytvoření databáze aplikace film za 15 minut s architekturou ASP.NET MVC (C#) | Microsoft Docs"
author: StephenWalther
description: "Stephen Walther vytváří celou databázových aplikací ASP.NET MVC aplikaci od začátku ukončíte. V tomto kurzu je skvělým Úvod pro osoby, které jsou nové t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: a67ca5422d4353b8c23b3fd804246906b8b6d717
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Vytvoření databáze aplikace film za 15 minut s architekturou ASP.NET MVC (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

[Stáhněte si kód](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther vytváří celou databázových aplikací ASP.NET MVC aplikaci od začátku ukončíte. V tomto kurzu je skvělým Úvod pro uživatele, kteří jsou nové rozhraní ASP.NET MVC a kteří chtějí získat představu o proces vytváření aplikace ASP.NET MVC.


Účelem tohoto kurzu je získáte představu o "co je jako" k vytvoření aplikace ASP.NET MVC. V tomto kurzu I vysokopecní prostřednictvím vytváření celou aplikaci ASP.NET MVC od začátku ukončíte. I ukazují, jak vytvořit jednoduchou aplikaci vycházející z databáze, která ukazuje, jak můžete zobrazit seznam, vytvářet a upravovat záznamy databáze.

Zjednodušit proces vytváření aplikace, můžeme vám využít výhod funkce generování uživatelského rozhraní sady Visual Studio 2008. Dáme Visual Studio generování počátečního kódu a obsahu pro naše řadiče, modely a zobrazení.

Pokud jste už pracovali s Active Server Pages nebo v technologii ASP.NET, pak byste měli najít ASP.NET MVC velmi seznámit. Zobrazení v rozhraní ASP.NET MVC jsou hodně podobá stránky v aplikaci Active Server Pages. A, stejně jako tradiční aplikace webových formulářů ASP.NET, rozhraní ASP.NET MVC poskytuje úplný přístup k širokou škálu jazyky a třídy poskytované rozhraní .NET framework.

Moje naděje je, že v tomto kurzu získáte představu o tom, jak postupu při vytváření aplikace ASP.NET MVC je podobné a jiné než možností vytvoření aplikace Active Server Pages nebo webových formulářů ASP.NET.

## <a name="overview-of-the-movie-database-application"></a>Přehled film databázové aplikace

Protože Naším cílem je zjednodušení, využijeme budete velmi jednoduchou aplikaci film databáze. Naše jednoduchou aplikaci film databáze umožní nám tři věci:

1. Seznam sadu záznamů databáze filmu
2. Vytvořit nový záznam v databázi filmu
3. Úpravy záznamů existující databáze filmu

Znovu protože chceme zjednodušení, provedeme výhod minimální počet funkcí rozhraní ASP.NET MVC framework potřebné k vytvoření aplikace. Například můžeme nebude využít výhod Test-Driven vývoj.

Chcete-li vytvořit aplikaci, je potřeba dokončit všechny následující kroky:

1. Vytvoření projektu webové aplikace ASP.NET MVC
2. Vytvoření databáze
3. Vytvořit model databáze.
4. Vytvoření řadiče ASP.NET MVC
5. Vytvoření zobrazení ASP.NET MVC

## <a name="preliminaries"></a>Nezbytné úkony

Budete potřebovat buď Visual Studio 2008 nebo Visual Web Developer 2008 Express k vytvoření aplikace ASP.NET MVC. Budete také muset stáhnout rozhraní ASP.NET MVC.

Pokud nevlastníte Visual Studio 2008, můžete stáhnout 90denní zkušební verzi sady Visual Studio 2008 z tohoto webu:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternativně můžete vytvořit rozhraní ASP.NET MVC aplikace s Visual Web Developer Express 2008. Pokud se rozhodnete použít Visual Web Developer Express musí mít nainstalovanou aktualizaci Service Pack 1. Visual Web Developer 2008 Express s aktualizací Service Pack 1 si můžete stáhnout z tohoto webu:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Po instalaci Visual Studio 2008 nebo Visual Web Developer 2008, musíte nainstalovat rozhraní ASP.NET MVC. Rozhraní ASP.NET MVC můžete stáhnout z následujícího webu:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Místo stažení rozhraní ASP.NET a rozhraní ASP.NET MVC jednotlivě, můžete využít výhod služby instalace webové platformy. Instalace webové platformy je aplikace, která umožňuje snadno spravovat nainstalované aplikace jsou v počítači:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Vytvoření projektu webové aplikace ASP.NET MVC

Začněme vytvořením nového projektu webové aplikace ASP.NET MVC v sadě Visual Studio 2008. Vyberte možnost nabídky **soubor, nový projekt** a zobrazí se dialogové okno Nový projekt na obrázku 1. Vyberte programovací jazyk C# a výběr šablony projektu webové aplikace ASP.NET MVC. Zadejte název MovieApp projektu a klikněte na tlačítko OK.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Obrázek 01**: dialogové okno Nový projekt ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


Zkontrolujte, vyberete rozhraní .NET Framework 3.5 z rozevíracího seznamu v horní části dialogového okna Nový projekt nebo v šabloně projektů webové aplikace ASP.NET MVC se nezobrazí.


Vždy, když vytvoříte nový projekt webové aplikace MVC, Visual Studio vyzve k vytvoření projektu testování částí samostatné. Otevře se dialogové okno na obrázku 2. Protože jsme nebude vytvářet testy v tomto kurzu z důvodu omezení času (a Ano, jsme by měl mít trochu dopustil o této) vyberte **ne** možnost a klikněte na tlačítko **OK** tlačítko.

> [!NOTE] 
> 
> Visual Web Developer nepodporuje projektů testů.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Obrázek 02**: dialogové okno Vytvoření projektu testování částí ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


Aplikace ASP.NET MVC je standardní sada složky: složku modely, zobrazení a Kontrolery. Zobrazí se tento standardní sadu složek v okně Průzkumníka řešení. Budeme potřebovat k přidání souborů do jednotlivých složek modely, zobrazení a Kontrolery k vytvoření databáze filmová aplikace.

Když vytvoříte nové aplikace MVC pomocí sady Visual Studio, můžete získat ukázkovou aplikaci. Vzhledem k tomu, že chceme začít úplně od začátku, je potřeba odstranit obsah této ukázkové aplikaci. Je třeba odstranit následující soubor a následující složku:

- Controllers\HomeController.cs
- Views\Home

## <a name="creating-the-database"></a>Vytvoření databáze

Je potřeba vytvořit databázi pro uložení našich záznamů film databáze. Naštěstí Visual Studio obsahuje bezplatná databáze s názvem serveru SQL Server Express. Postupujte podle těchto kroků můžete vytvořit databázi:

1. Klikněte pravým tlačítkem na aplikaci\_složky dat v okna Průzkumníka řešení a vyberte možnost nabídky **přidat, nové položky**.
2. Vyberte **Data** kategorie a vyberte **databáze systému SQL Server** šablony (viz obrázek 3).
3. Název nové databáze *MoviesDB.mdf* a klikněte na **přidat** tlačítko.

Po vytvoření databáze, se můžete připojit k databázi pomocí dvojitým kliknutím na soubor MoviesDB.mdf umístěný v aplikaci\_složku Data. Dvojitým kliknutím na soubor MoviesDB.mdf otevře okno Průzkumníka serveru.

> [!NOTE] 
> 
> Okno Průzkumníka serveru jmenuje okno Průzkumník databáze v případě Visual Web Developer.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Obrázek 03**: vytvoření databáze Microsoft SQL Server ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


Dále je potřeba vytvořit novou tabulku databáze. V okně Průzkumníka serveru, klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**. Výběrem této možnosti nabídky otevře návrháře tabulky databáze. Vytvořte následující sloupců databáze:

<a id="0.1_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | celá čísla | False |
| Název | Nvarchar(100) | False |
| Adresář nacházející | Nvarchar(100) | False |
| DateReleased | DateTime | False |


První sloupec, ve sloupci Id má dvě speciální vlastnosti. Nejprve budete muset označte Id sloupec jako sloupec primárního klíče. Po výběru Id sloupce, klikněte na **nastavit primární klíč** tlačítko (je ikona, která vypadá jako klíč). Druhý je nutné označit Id sloupec jako sloupec Identity. V okně Vlastnosti sloupce přejděte do části specifikace Identity a rozbalte ho. Změna **je Identity** vlastnost na hodnotu **Ano**. Jakmile budete hotovi, v tabulce by měl vypadat jako na obrázku 4.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Obrázek 04**: filmy databázové tabulky ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


Posledním krokem je uložit do nové tabulky. Klikněte na tlačítko Uložit (ikona disketová) a poskytněte novou tabulku filmy název.

Po dokončení vytváření tabulky, přidejte některé záznamy film do tabulky. Klikněte pravým tlačítkem na filmy tabulkou v okně Průzkumníka serveru a vyberte možnost nabídky **zobrazit Data tabulky**. Zadejte seznam vaše oblíbené filmy (viz obrázek 5).


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Obrázek 05**: zadávání film záznamy ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>Vytvoření modelu

Další potřebujeme vytvořit sadu třídy představující naše databáze. Potřebujeme vytvořit model databáze. Provedeme výhod Entity Framework Microsoft automaticky vygenerovat třídy pro náš model databáze.

> [!NOTE] 
> 
> Rozhraní ASP.NET MVC není vázaný Microsoft Entity Framework. Můžete využívat výhod celou řadu relační mapování objektu databázi vytvořit třídy modelu (nebo / M) včetně technologie LINQ to SQL, Subsonic a NHibernate nástroje.


Postupujte podle těchto kroků spusťte průvodce Entity Data Model:

1. Klikněte pravým tlačítkem na složku modely v okně Průzkumníka řešení a vyberte možnost nabídky **přidat, nové položky**.
2. Vyberte **Data** kategorie a vyberte **ADO.NET Entity Data Model** šablony.
3. Zadejte název datového modelu *MoviesDBModel.edmx* a klikněte na tlačítko **přidat** tlačítko.

Po kliknutí na tlačítko Přidat, zobrazí se průvodce Entity Data Model (viz obrázek 6). Postupujte podle následujících kroků dokončete průvodce:

1. V **zvolte obsah modelu** krok, vyberte **generování z databáze** možnost.
2. V **vybrat datové připojení** krok, použijte *MoviesDB.mdf* datové připojení a názvu *MoviesDBEntities* pro nastavení připojení. Klikněte **Další** tlačítko.
3. V **zvolte si databázové objekty** krok, rozbalte uzel tabulky, vyberte tabulku, filmy. Zadejte obor názvů *MovieApp.Models* a klikněte na **Dokončit** tlačítko.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Obrázek 06**: generování modelu databáze s průvodci Entity Data Model ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Po dokončení průvodce Entity Data Model Entity Data Model Designer se otevře. Návrhář by měl zobrazit filmy databázové tabulky (viz obrázek 7).


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Obrázek 07**: Entity Data Model Designer ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


Je potřeba jeden změnu provést, abychom mohli pokračovat. Průvodce dat Entity vygeneruje třídu modelu s názvem *filmy* představující filmy databázové tabulky. Protože třída filmy použijeme k reprezentaci konkrétní film, je potřeba změnit název třídy, která má být *film* místo *filmy* (singulární spíše než množném čísle).

Dvakrát klikněte na název třídy na plochu návrháře a změňte název třídy z filmy film. Po provedení této změny, klikněte **Uložit** tlačítko (ikona disketu) ke generování film třídy.

## <a name="creating-the-aspnet-mvc-controller"></a>Vytvoření řadiče rozhraní ASP.NET MVC

Dalším krokem je vytvoření kontroleru rozhraní ASP.NET MVC. Řadič slouží k řízení, jak se uživatel pracuje s aplikaci ASP.NET MVC.

Postupujte podle těchto kroků:

1. V okně Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, řadiče**.
2. V dialogovém okně Přidat kontroler, zadejte název *HomeController* a zaškrtněte políčko s názvem bez přípony **přidejte metody akce pro vytvoření, aktualizace a podrobnosti o scénářích** (viz obrázek 8).
3. Klikněte **přidat** tlačítko přidáte nový řadič do projektu.

Po dokončení těchto kroků se vytvoří řadiče v výpis 1. Všimněte si, že obsahuje metody s názvem Index, podrobnosti, vytvořit a upravte. V následujících částech přidáme nezbytné kód slouží k získání těchto metod pro práci.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Obrázek 08**: přidávání nového řadiče MVC ASP.NET ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**Výpis 1 – Controllers\HomeController.cs**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Seznam záznamů databáze

Metoda Index() řadiče domovské je výchozí metodou pro aplikaci ASP.NET MVC. Při spuštění aplikace ASP.NET MVC je metoda Index() první řadič metodu, která je volána.

Chcete-li zobrazit seznam záznamů z tabulky databáze filmy použijeme metodu Index(). Provedeme výhod databáze třídy modelu, které jsme vytvořili předtím pro načtení záznamů databáze film s metodou Index().

Třídy HomeController ve výpisu 2 jste došlo tak, aby obsahoval nové privátní pole s názvem \_db. Třída MoviesDBEntities představuje naše model databáze a tato třída použijeme ke komunikaci s naše databází.

Také jste došlo metodu Index() v výpis 2. Metoda Index() používá třídu MoviesDBEntities načíst všechny záznamy film z filmy databázové tabulky. Výraz  *\_db. MovieSet.ToList()* vrátí seznam všech záznamů film z filmy databázové tabulky.

Seznam filmy, je předaná do zobrazení. Všechno, co získá předaný metodě View() předána do zobrazení jako data zobrazení.

**Výpis 2 – Controllers/HomeController.cs (upravené Index metoda)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

Vrátí metoda Index() zobrazení s názvem Index. Je potřeba vytvořit seznam záznamů databáze film tohoto zobrazení. Postupujte podle těchto kroků:


Má sestavit projekt (vyberte možnost nabídky **sestavení, sestavit řešení**) před otevřením **přidat zobrazení** dialogového nebo žádné třídy, zobrazí se v **zobrazit třída dat** rozevírací seznam.


1. Klikněte pravým tlačítkem na metodu Index() v editoru kódu a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 9).
2. V dialogovém okně Přidat zobrazení, ověřte, že políčko s názvem bez přípony **vytvořit zobrazení silného typu** je zaškrtnuté.
3. Z **zobrazit obsah** rozevíracího seznamu, vyberte hodnotu *seznamu*.
4. Z **zobrazit třída dat** rozevíracího seznamu, vyberte hodnotu *MovieApp.Models.Movie*.
5. Kliknutím na tlačítko Přidat k vytvoření nové zobrazení (viz obrázek 10).

Po dokončení těchto kroků nové zobrazení s názvem Index.aspx přidán do složky Views\Home. Obsah zobrazení indexu jsou součástí výpis 3.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Obrázek 09**: přidávání zobrazení z akce kontroleru ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Obrázek 10**: vytvoření nové zobrazení pomocí dialogu přidat zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**Listing 3 – Views\Home\Index.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

Zobrazení indexu zobrazuje všechny záznamy filmu ze filmy databázové tabulky v rámci tabulky HTML. Zobrazení obsahuje smyčka typu foreach, který iteruje v rámci každé film reprezentována ViewData.Model vlastnost. Pokud vaše aplikace spouštěné stiskne klávesu F5, uvidíte webové stránky v obrázek 11.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Obrázek 11**: Zobrazení indexu ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>Vytváření nových záznamů databáze

Index zobrazení, které jsme vytvořili v předchozí části obsahuje odkaz pro vytvoření nové databáze záznamy. Vraťme se dopředu a implementovat logiku a vytvořte potřebné pro vytvoření nové databáze záznamy film zobrazení.

Řadičem domovské obsahuje dvě metody s názvem Create(). První metoda Create() nemá žádné parametry. Toto přetížení metody Create() se používá k zobrazení formuláře HTML pro vytvoření nového záznamu film databáze.

Druhá metoda Create() obsahuje parametr FormCollection. Toto přetížení metody Create() je volána při odeslání formuláře HTML pro vytvoření nové film k serveru. Všimněte si, že tato metoda Create() má AcceptVerbs atribut, který brání metoda volána, pokud se provádí operaci HTTP POST.

Tato metoda Create() se změnilo ve třídě HomeController aktualizovaný seznam 4. Nová verze metody Create() přijme parametr film a obsahuje logiku pro vkládání nové film do tabulky databáze filmy.

> [!NOTE] 
> 
> Všimněte si atribut vazby. Vzhledem k tomu, že jsme nechcete aktualizovat vlastnost Id film z formuláře HTML, je potřeba explicitně vyloučit tuto vlastnost.


**Výpis 4 – Controllers\HomeController.cs (upravené metodu Create)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio usnadňuje vytvoření formuláře pro vytvoření nové databáze film záznam (viz obrázek 12). Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na metodu Create() v editoru kódu a vyberte možnost nabídky **přidat zobrazení**.
2. Ověřte, že políčko s názvem bez přípony **vytvořit zobrazení silného typu** je zaškrtnuté.
3. Z **zobrazit obsah** rozevíracího seznamu, vyberte hodnotu *vytvořit*.
4. Z **zobrazit třída dat** rozevíracího seznamu, vyberte hodnotu *MovieApp.Models.Movie*.
5. Klikněte **přidat** tlačítko vytvořte nové zobrazení.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Obrázek 12**: Přidání zobrazení pro vytváření ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


Visual Studio automaticky generuje zobrazení v výpis 5. Toto zobrazení obsahuje formuláře HTML, která obsahuje pole, které odpovídají každé z vlastností třídy film.

**Listing 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> Formuláře HTML vygenerované pomocí dialogového okna Přidat zobrazení generuje pole Id formuláře. Protože Id sloupec je sloupec Identity, potřebujeme není toto pole formuláře a je možné bezpečně odebrat.


Po přidání zobrazení pro vytváření, můžete přidat nové záznamy film do databáze. Spusťte aplikaci stisknutím klávesy F5 a klikněte na tlačítko Vytvořit nový odkaz zobrazíte formuláře v obrázek 13. Pokud dokončení a odeslání formuláře se vytvoří nový záznam v databázi film.

Všimněte si, automaticky získat ověřování formuláře. Pokud neprovedete zadejte datum vydání pro film, nebo můžete zadat datum neplatná verze, formulář se zobrazí znovu a zvýrazní pole pro datum vydání.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Obrázek 13**: vytváření nového záznamu databáze movie ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>Úpravou existujících záznamů databáze

V předchozích částech jsme probrali, jak můžete seznam a vytvořit nové záznamy v databázi. V této poslední části probereme, jak můžete upravit existující záznamy databáze.

Nejprve musíme Generovat formulář upravit. Tento krok je snadné, protože Visual Studio bude formulář upravit pro nás automaticky generovat. Otevřete HomeController.cs třídy v sadě Visual Studio editoru kódu a postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na metodu Edit() v editoru kódu a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 14).
2. Zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu**.
3. Z **zobrazit obsah** rozevíracího seznamu, vyberte hodnotu *upravit*.
4. Z **zobrazit třída dat** rozevíracího seznamu, vyberte hodnotu *MovieApp.Models.Movie*.
5. Klikněte **přidat** tlačítko vytvořte nové zobrazení.

Dokončení těchto kroků přidá nové zobrazení s názvem Edit.aspx ke složce Views\Home. Toto zobrazení obsahuje formuláře HTML pro úpravu záznamu film.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Obrázek 14**: Přidání zobrazení upravit ([Kliknutím zobrazit obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> Upravit zobrazení obsahuje pole formuláře HTML, která odpovídá Id film vlastnost. Vzhledem k tomu, že nechcete, aby lidé úpravy hodnotu vlastnosti Id, byste měli odebrat toto pole formuláře.


Nakonec je potřeba upravit řadičem domovské tak, aby podporoval úprava záznamů databáze. Aktualizované třídy HomeController je součástí výpis 6.

**Výpis 6 – Controllers\HomeController.cs (upravit metody)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

Ve výpisu 6 byly přidány další logiku do obou přetížení metody Edit(). První metoda Edit() vrátí záznam databáze film, která odpovídá Id parametr předaný metodě. Druhý přetížení provádí aktualizace na film záznam v databázi.

Všimněte si, že je nutné získat původní film a pak zavolají ApplyPropertyChanges(), aktualizovat existující film v databázi.

## <a name="summary"></a>Souhrn

Účelem tohoto kurzu bylo získáte představu o postupu při vytváření aplikace ASP.NET MVC. Ať zjistit, že vytváření webové aplikace ASP.NET MVC je velmi podobný postupu při vytvoření aplikace Active Server Pages nebo ASP.NET.

V tomto kurzu jsme se zaměřili pouze nejzákladnější funkce rozhraní ASP.NET MVC. V budoucích kurzech jsme podrobně hlubší oblastech, jako je řadiče, akce kontroleru, zobrazení, data zobrazení a pomocné rutiny HTML.

>[!div class="step-by-step"]
[Next](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
