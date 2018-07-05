---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: Vytvoření aplikace Movie Database za 15 minut s ASP.NET MVC (VB) | Dokumentace Microsoftu
author: StephenWalther
description: Stephen Walther sestavení celý databázově řízeného aplikaci ASP.NET MVC od začátku na dokončení. Tento kurz představuje vynikající Úvod pro uživatele, kteří jsou nové t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: a64d140eba4ebf489486af1a0be6269a8b904c13
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372592"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>Vytvoření aplikace Movie Database za 15 minut s ASP.NET MVC (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

[Stáhnout kód](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther sestavení celý databázově řízeného aplikaci ASP.NET MVC od začátku na dokončení. Tento kurz představuje vynikající Úvod pro lidi, kteří jsou nové rozhraní ASP.NET MVC a kteří chtějí získat představu o procesu vytvoření aplikace ASP.NET MVC.


Účelem tohoto kurzu je získáte představu o "co je třeba" pro vytvoření aplikace ASP.NET MVC. V tomto kurzu můžu vysokopecní prostřednictvím vytváření celé aplikace ASP.NET MVC od začátku na dokončení. Můžu ukazují, jak vytvořit jednoduchou aplikaci databázově řízeného, která ukazuje, jak můžete seznamu, vytvářet a upravovat záznamy databáze.

Chcete-li zjednodušit proces vytváření aplikace, provedeme výhod funkcí generování uživatelského rozhraní sady Visual Studio 2008. Dáme vám Visual Studio generování počátečního kódu a obsah pro naše řadiče, modely a zobrazení.

Pokud jste už pracovali s ASP nebo ASP.NET, pak byste měli najít ASP.NET MVC povědomý. Zobrazení ASP.NET MVC jsou velmi podobně jako na stránkách v aplikaci ASP. A stejně jako tradiční aplikace webových formulářů ASP.NET, ASP.NET MVC poskytuje úplný přístup k bohaté sadě jazyků a tříd poskytovaných rozhraním .NET framework.

Moje naděje je, že v tomto kurzu získáte představu o způsobu vytváření aplikací aplikace ASP.NET MVC je podobné a jiné než vytváření aplikací ASP nebo ASP.NET webové formuláře aplikace.

## <a name="overview-of-the-movie-database-application"></a>Přehled aplikace Movie Database

Protože Naším cílem je kvůli zjednodušení, vytvoříme velmi jednoduchá aplikace Movie Database. Naše jednoduché aplikace Movie Database vám umožní nám to udělat tři věci:

1. Seznam sadu záznamů databáze filmů
2. Vytvoří nový záznam databáze filmů
3. Upravit existující záznam databáze filmů

Znovu protože chceme, abychom si to nekomplikovali, provedeme výhod minimální počet funkcí rozhraní ASP.NET MVC, která je potřebná pro vytváření naši aplikaci. Například společnost Microsoft nebude využít výhod Test-Driven vývoje.

Chcete-li vytvořit aplikaci, musíme provést všechny následující kroky:

1. Vytvoření projektu webové aplikace ASP.NET MVC
2. Vytvoření databáze
3. Vytvoření databáze modelu
4. Vytvoří kontroler ASP.NET MVC
5. Vytvořit zobrazení ASP.NET MVC

## <a name="preliminaries"></a>Nezbytné úkony

Budete potřebovat Visual Studio 2008 nebo Visual Web Developer 2008 Express k sestavení aplikace ASP.NET MVC. Budete také muset stáhnout rozhraní ASP.NET MVC.

Pokud nevlastníte Visual Studio 2008, můžete z tohoto webu stažení 90denní zkušební verzi sady Visual Studio 2008:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternativně můžete vytvořit ASP.NET MVC aplikace Visual Web Developer Express 2008. Pokud se rozhodnete použít aplikaci Visual Web Developer Express musí mít nainstalovanou aktualizaci Service Pack 1. Visual Web Developer 2008 Express s aktualizací Service Pack 1 si můžete stáhnout z tohoto webu:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Po instalaci sady Visual Studio 2008 nebo Visual Web Developer 2008, musíte nainstalovat rozhraní ASP.NET MVC. Architektura ASP.NET MVC si můžete stáhnout z následujícího webu:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Místo stažení rozhraní ASP.NET a architektura ASP.NET MVC jednotlivě, můžete využít instalačního programu webové platformy. Web Platform Installer je v počítači jsou aplikace, která umožňuje snadno spravovat nainstalované aplikace:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Vytvoření projektu webové aplikace ASP.NET MVC

Začněme tím, že vytvoříte nový projekt webové aplikace ASP.NET MVC v sadě Visual Studio 2008. Vyberte možnost nabídky **soubor, nový projekt** a zobrazí se dialogové okno Nový projekt na obrázku 1. Vyberte jako programovací jazyk Visual Basic a vyberte šablonu projektu webové aplikace ASP.NET MVC. Dejte projektu název MovieApp a klikněte na tlačítko OK.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Obrázek 01**: dialogové okno Nový projekt ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


Ujistěte se, že vyberete rozhraní .NET Framework 3.5 z rozevíracího seznamu v horní části dialogového okna Nový projekt nebo šablonu projektu webové aplikace ASP.NET MVC se nezobrazí.


Pokaždé, když vytvoříte nový projekt webové aplikace MVC, Visual Studio vás vyzve k vytvoření samostatné jednotky testovacího projektu. Zobrazí se dialogové okno na obrázku 2. Protože společnost Microsoft nebude vytvářet testy v tomto kurzu z důvodu omezení času (a, Ano, by měl že o to trochu dopustil) vyberte **ne** možnost a klikněte na tlačítko **OK** tlačítko.

> [!NOTE] 
> 
> Visual Web Developer nepodporuje projekty testů.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Obrázek 02**: dialogové okno pro vytvoření projektu testů jednotek ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


Aplikace ASP.NET MVC je standardní sadu složky: složka modelů, zobrazení a Kontrolerů. Zobrazí se tento standardní sadu složek v okně Průzkumník řešení. Potřebujeme přidat soubory do složky modelů, zobrazení a Kontrolerů k vytvoření aplikace Movie Database.

Při vytváření nové aplikace MVC pomocí sady Visual Studio získáte ukázkovou aplikaci. Protože chceme začít úplně od začátku, musíme odstranit obsah této ukázkové aplikaci. Je třeba odstranit následující soubor a do následující složky:

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Vytvoření databáze

Potřebujeme vytvořit databázi pro uložení našich záznamů databáze filmů. Naštěstí sady Visual Studio zahrnuje bezplatné databázi s názvem serveru SQL Server Express. Postupujte podle těchto kroků a vytvořte databázi:

1. Klikněte pravým tlačítkem na aplikaci\_složce dat v okně Průzkumníka řešení a vyberte možnost nabídky **přidat, nová položka**.
2. Vyberte **Data** kategorie a vyberte **databázi systému SQL Server** šablony (viz obrázek 3).
3. Název nové databáze *MoviesDB.mdf* a klikněte na tlačítko **přidat** tlačítko.

Po vytvoření databáze se můžete připojit k databázi dvojitým kliknutím MoviesDB.mdf soubor umístěný v aplikaci\_složku Data. Dvojitým kliknutím na soubor MoviesDB.mdf otevře okno Průzkumníka serveru.

> [!NOTE] 
> 
> V okně Průzkumníka serveru má název okna Průzkumníka databáze v případě Visual Web Developer.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Obrázek 03**: vytvoření databáze Microsoft SQL Server ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


Dále musíme vytvořit nové databázové tabulky. Z okna Průzkumníka serveru, klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**. Výběrem této možnosti nabídky, otevře se Návrhář tabulky databáze. Vytvořte tyto sloupce databáze:

<a id="0.2_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | int | False |
| Název | Nvarchar(100) | False |
| Ředitel | Nvarchar(100) | False |
| DateReleased | DateTime | False |


První sloupec sloupec Id má dva speciální vlastnosti. Nejprve budete muset označit Id sloupec jako sloupec primárního klíče. Až vyberete sloupec Id, klikněte na tlačítko **nastavit primární klíč** tlačítko (je ikona, která vypadá jako klíče). Za druhé budete muset označit Id sloupec jako sloupec Identity. V okně Vlastnosti sloupce posuňte se dolů k části specifikace Identity a rozbalte ho. Změnit **je identita** k hodnotě **Ano**. Až budete hotovi, v tabulce by měl vypadat jako obrázek 4.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Obrázek 04**: filmy databázové tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


Posledním krokem je uložit do nové tabulky. Klikněte na tlačítko Save (ikona disketová) a poskytnout filmy název nové tabulky.

Po dokončení vytváření tabulky přidáte některé záznamy video do tabulky. Klikněte pravým tlačítkem na filmy tabulce v okně Průzkumníka serveru a vyberte možnost nabídky **zobrazit Data tabulky**. Zadejte seznam oblíbených video (viz obrázek 5).


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Obrázek 05**: zadání film záznamů ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>Vytvoření modelu

Dále musíme vytvořit sadu tříd představující naší databázi. Potřebujeme vytvořit model databáze. Provedeme výhod Microsoft Entity Framework k automatickému vygenerování třídy pro náš model databáze.

> [!NOTE] 
> 
> Architektura ASP.NET MVC se neváže na Microsoft Entity Framework. Můžete vytvořit databázi model třídy s využitím různých relační mapování objektů (nebo / M) nástroje, včetně technologie LINQ to SQL, Subsonic a NHibernate.


Použijte následující postup spuštění Průvodce datovým modelem Entity:

1. Klikněte pravým tlačítkem na složku modely v okně Průzkumník řešení a vyberte možnost nabídky **přidat, nová položka**.
2. Vyberte **Data** kategorie a vyberte **datový Model Entity ADO.NET** šablony.
3. Zadejte název datového modelu *MoviesDBModel.edmx* a klikněte na tlačítko **přidat** tlačítko.

Po kliknutí na tlačítko Přidat, zobrazí se Průvodce datovým modelem Entity (viz obrázek 6). Postupujte podle následujících kroků dokončete průvodce:

1. V **výběr obsahu modelu** kroku, vyberte **Generovat z databáze** možnost.
2. V **vyberte datové připojení** kroku, použijte *MoviesDB.mdf* datové připojení a názvu *MoviesDBEntities* pro nastavení připojení. Klikněte na tlačítko **Další** tlačítko.
3. V **zvolte vaše databázové objekty** krok, rozbalte uzel tabulky, vyberte v tabulce videa. Zadejte obor názvů *MovieApp.Models* a klikněte na tlačítko **Dokončit** tlačítko.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Obrázek 06**: generování modelu databáze se Průvodce datovým modelem Entity ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


Po dokončení Průvodce datovým modelem Entity, otevře se Návrhář Entity Data Model. Návrhář zobrazeno v tabulce databáze filmů (viz obrázek 7).


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Obrázek 07**: Entity Data Model Designer ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


Potřebujeme, aby jednu změnu, abychom mohli pokračovat. Průvodce Entity Data vygeneruje třídu modelu s názvem videa, která představuje filmy databázové tabulky. Vzhledem k tomu použijeme filmy třídy představující konkrétní videa, potřeba změnit název třídy, která má být *film* místo *filmy* (singulární spíše než množné číslo).

Dvakrát klikněte na název třídy na návrhové ploše a změňte název třídy z videa na video. Po provedení této změny, klikněte na tlačítko **Uložit** tlačítko (ikona disketu) ke generování třídy Video.

## <a name="creating-the-aspnet-mvc-controller"></a>Vytvoření Kontroleru rozhraní ASP.NET MVC

Dalším krokem je vytvoření kontroler ASP.NET MVC. Kontroler je zodpovědná za řízení interakce uživatele s aplikací ASP.NET MVC.

Postupujte podle těchto kroků:

1. V okně Průzkumníka řešení, klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, řadič**.
2. V dialogovém okně Přidat kontroler, zadejte název *HomeController* a zaškrtněte políčko s popiskem **přidejte metody akce pro vytvoření, aktualizace a podrobnosti o scénářích** (viz obrázek 8).
3. Klikněte na tlačítko **přidat** tlačítko pro přidání do projektu nový kontroler.

Po dokončení těchto kroků se vytvoří kontroler v informacích 1. Všimněte si, že obsahuje metody s názvem Index, podrobností, vytvořit a upravit. V následujících částech přidáme nezbytné kódu ke splnění tyto metody pro práci.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Obrázek 08**: přidáte nový kontroler ASP.NET MVC ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**Výpis 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Výpis záznamů databáze

Metoda Index() kontroler Home je výchozí metodou pro aplikace ASP.NET MVC. Při spuštění aplikace ASP.NET MVC je metoda Index() první, která je volána metoda kontroleru.

Chcete-li zobrazit seznam záznamů v tabulce databáze filmů použijeme metodu Index(). Provedeme výhod databáze třídy modelu, které jsme vytvořili dříve pro načtení záznamů databáze filmů Index() metoda.

Můžu HomeController třídy ve výpisu 2 upravili tak, aby obsahoval nový soukromé pole s názvem \_db. Představuje třídu MoviesDBEntities náš model databáze a použijeme tuto třídu komunikovat s naší databází.

Můžu jsme také upravit metoda Index() ve výpisu 2. Metoda Index() používá třídu MoviesDBEntities načíst všechny záznamy video z databázové tabulky videa. Výraz  *\_db. MovieSet.ToList()* vrátí seznam všech záznamů film z databázové tabulky videa.

Seznam filmy předána do zobrazení. Cokoli, co bude předána metodě View() bude předána do zobrazení jako data zobrazení.

**Výpis 2 – Controllers/HomeController.vb (upravená metoda indexu)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Metoda Index() vrátí zobrazení s názvem Index. Potřebujeme vytvořit toto zobrazení zobrazí seznam záznamů databáze filmů. Postupujte podle těchto kroků:


Váš projekt má sestavit (vyberte možnost nabídky **vytvořit, sestavit řešení**) před otevřením **přidat zobrazení** se zobrazí dialogové okno nebo žádné třídy v **zobrazení dat třídy** rozevírací seznam.


1. Klikněte pravým tlačítkem na metodu Index() v editoru kódu a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 9).
2. V dialogovém okně Přidat zobrazení, ověřte, že označení zaškrtávacího políčka **vytvoření zobrazení se silnými typy** je zaškrtnuté políčko.
3. Z **zobrazit obsah** rozevíracího seznamu vyberte hodnotu *seznamu*.
4. Z **zobrazení dat třídy** rozevíracího seznamu vyberte hodnotu *MovieApp.Movie*.
5. Klikněte na tlačítko Přidat pro vytvoření nového zobrazení (viz obrázek 10).

Po dokončení těchto kroků se přidá nová zobrazení s názvem Index.aspx ke složce Views\Home. Obsah zobrazení indexu jsou součástí výpis 3.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Obrázek 09**: Přidání zobrazení z kontroleru akce ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Obrázek 10**: vytvoření nového zobrazení se dialogové okno Přidat zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

Index zobrazení zobrazí všechny záznamy video z databázové tabulky videa v rámci tabulku HTML. Obsahuje zobrazení pro každou smyčku, která prochází každý film reprezentována ViewData.Model vlastností. Pokud spustíte svou aplikaci stisknutím klávesy F5, uvidíte webové stránky v obrázek 11.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Obrázek 11**: Index zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>Vytváření nových záznamů databáze

Index zobrazení, které jsme vytvořili v předchozí části obsahuje odkaz pro vytvoření nové záznamy v databázi. Nyní pokračujme a implementovat logiku a vytvořit zobrazení pro vytváření nových záznamů databáze filmů.

Kontroler Home obsahuje dvě metody s názvem Create(). První metoda Create() nemá žádné parametry. Toto přetížení metody Create() slouží k zobrazení formuláře HTML pro vytvoření nového záznamu v databázi video.

Druhá metoda Create() má parametr FormCollection. Toto přetížení metody Create() se volá při odeslání formuláře HTML pro vytvoření nového videa na server. Všimněte si, že tato metoda Create() má AcceptVerbs atribut, který brání metoda volána, pokud se provádí operaci HTTP Post.

Tento druhý způsob Create() se upravil ve třídě HomeController aktualizovaný seznam 4. Nová verze Create() metoda přijímá parametr filmů a obsahuje logiku pro vkládání do tabulky databáze filmů nové video.

> [!NOTE] 
> 
> Všimněte si, že atribut vazby. Protože nechceme umožňuje aktualizovat vlastnost Id video z formuláře HTML, je potřeba explicitně vyloučit tuto vlastnost.


**Část 4 – Controllers\HomeController.vb (upravená metoda vytvořit)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio umožňuje snadno vytvořit formulář pro vytvoření nové databáze filmů záznam (viz obrázek 12). Postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na metodu Create() v editoru kódu a vyberte možnost nabídky **přidat zobrazení**.
2. Ověřte, že označení zaškrtávacího políčka **vytvoření zobrazení se silnými typy** je zaškrtnuté políčko.
3. Z **zobrazit obsah** rozevíracího seznamu vyberte hodnotu *vytvořit*.
4. Z **zobrazení dat třídy** rozevíracího seznamu vyberte hodnotu *MovieApp.Movie*.
5. Klikněte na tlačítko **přidat** pro vytvoření nového zobrazení.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Obrázek 12**: Přidání zobrazení pro vytváření ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Visual Studio automaticky vytvoří zobrazení v informacích 5. Toto zobrazení obsahuje formuláře HTML, který obsahuje pole, které odpovídají jednotlivým vlastnosti třídy Video.

**Výpis 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Formulář HTML generovaných dialogové okno Přidat zobrazení generuje pole Id formuláře. Vzhledem k tomu, že sloupec Id je sloupec Identity, nebudeme potřebovat toto pole formuláře a můžete ho bezpečně odebrat.


Po přidání zobrazení pro vytváření, můžete přidat nové video záznamy do databáze. Stisknutím klávesy F5 spusťte aplikaci a klikněte na tlačítko na formulář v nástrojích pro obrázek 13 vytvořit nový odkaz. Pokud dokončení a odeslání formuláře se vytvoří nový záznam v databázi video.

Všimněte si, že automaticky získání ověřovacího formuláře. Pokud opomenete zadejte datum vydání verze pro video, nebo zadejte datum neplatný vydání, formulář se zobrazí znovu a je zvýrazněn pole Datum vydání verze.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Obrázek 13**: vytváří se nový záznam v databázi movie ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>Úpravy existujících záznamů v databázi

V předchozích částech jsme probírali jak seznamu a vytvořit nové záznamy v databázi. V této závěrečné sekci si popíšeme, jak můžete upravit existujících záznamů v databázi.

Nejdřív potřebujeme Generovat formulář pro úpravy. Tento krok je jednoduchý, protože Visual Studio vygeneruje formulář pro úpravy pro nás automaticky. Otevřete třídu HomeController.vb v editoru kódu sady Visual Studio a postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na metodu Edit() v editoru kódu a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 14).
2. Zaškrtněte políčko s popiskem **vytvoření zobrazení se silnými typy**.
3. Z **zobrazit obsah** rozevíracího seznamu vyberte hodnotu *upravit*.
4. Z **zobrazení dat třídy** rozevíracího seznamu vyberte hodnotu *MovieApp.Movie*.
5. Klikněte na tlačítko **přidat** pro vytvoření nového zobrazení.

Následující postup přidá nové zobrazení s názvem Edit.aspx ke složce Views\Home. Toto zobrazení obsahuje formulář pro úpravy záznamu video ve formátu HTML.


[![Dialogové okno Nový projekt](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Obrázek 14**: Přidání zobrazení pro úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> Zobrazení pro úpravy obsahuje pole formuláře HTML, který odpovídá vlastnosti Id video. Vzhledem k tomu, že chcete lidem zakázat úpravy hodnoty vlastnosti Id, měli byste odebrat toto pole formuláře.


Nakonec musíme upravit kontroler Home tak, aby podporoval úpravy záznamu v databázi. Aktualizované HomeController třídy jsou obsaženy v informacích 6.

**Výpis 6 – Controllers\HomeController.vb (metod Edit)**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

V zobrazení 6 můžu pro obě přetížení metody Edit() přidali další logiku. První metoda Edit() vrátí záznam databáze filmů, která odpovídá předat parametru Id předaný metodě. Druhé přetížení provádí aktualizace na video záznam v databázi.

Všimněte si, že je nutné získat původní video a pak zavolat ApplyPropertyChanges() aktualizovat existující videa v databázi.

## <a name="summary"></a>Souhrn

Účelem tohoto kurzu byla získáte představu o vytváření aplikací aplikace ASP.NET MVC. Doufám, že jste zjistili, že sestavení webové aplikace ASP.NET MVC je velmi podobné vytváření aplikací aplikace ASP nebo ASP.NET.

V tomto kurzu jsme se zaměřili na pouze základní funkce rozhraní ASP.NET MVC. V budoucích kurzech se budeme podrobněji věnovat, jako jsou řadiče, akce kontroleru, zobrazení, zobrazení dat a pomocných rutin HTML.

> [!div class="step-by-step"]
> [Předchozí](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
