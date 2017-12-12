---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: "Zobrazení tabulky dat databáze (VB) | Microsoft Docs"
author: microsoft
description: "V tomto kurzu ukazují I dvě metody zobrazení sadu záznamů databáze. Zobrazit dvě metody formátování sadu záznamů databáze v kódu HTML tových..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dc0aa91cfb68d308ed098f429d3251d424ab778
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-vb"></a>Zobrazení tabulky dat databáze (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> V tomto kurzu ukazují I dvě metody zobrazení sadu záznamů databáze. Zobrazit dvě metody formátování sadu záznamů databáze v tabulce jazyka HTML. Nejprve I ukazují použití formátu záznamů databáze přímo v rámci zobrazení. V dalším kroku I ukazují, jak můžete využít výhod částečné při formátování záznamů databáze.


Cílem tohoto kurzu je vysvětlují, jak můžete zobrazit tabulky HTML dat z databáze v aplikaci ASP.NET MVC. První můžete další informace o použití nástrojů generování uživatelského rozhraní v sadě Visual Studio ke generování zobrazení, které se automaticky zobrazí sadu záznamů. Dále můžete další informace o použití částečné jako šablonu při formátování záznamů databáze.

## <a name="create-the-model-classes"></a>Vytvoření třídy modelu

Nyní klikněte na Zobrazit sadu záznamů z filmy databázové tabulky. V tabulce databáze filmy obsahuje následující sloupce:

<a id="0.4_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | celá čísla | False |
| Název | Nvarchar(200) | False |
| Adresář nacházející | NVarchar(50) | False |
| DateReleased | DateTime | False |


Aby mohl představovat filmy tabulky v naší aplikaci ASP.NET MVC, musíme vytvořit třídu modelu. V tomto kurzu používáme k vytváření tříd naše modelu Microsoft Entity Framework.

> [!NOTE] 
> 
> V tomto kurzu používáme Microsoft Entity Framework. Je ale důležité si uvědomit, že můžete použít řadu různých technologií pro interakci s databází z aplikace ASP.NET MVC, včetně technologie LINQ to SQL, NHibernate nebo ADO.NET.


Postupujte podle těchto kroků spusťte průvodce Entity Data Model:

1. Klikněte pravým tlačítkem na složku modely v okně Průzkumníka řešení a vyberte možnost nabídky **přidat, nové položky**.
2. Vyberte **Data** kategorie a vyberte **ADO.NET Entity Data Model** šablony.
3. Zadejte název datového modelu *MoviesDBModel.edmx* a klikněte na tlačítko **přidat** tlačítko.

Po kliknutí na tlačítko Přidat, zobrazí se průvodce Entity Data Model (viz obrázek 1). Postupujte podle následujících kroků dokončete průvodce:

1. V **zvolte obsah modelu** krok, vyberte **generování z databáze** možnost.
2. V **vybrat datové připojení** krok, použijte *MoviesDB.mdf* datové připojení a názvu *MoviesDBEntities* pro nastavení připojení. Klikněte **Další** tlačítko.
3. V **zvolte si databázové objekty** krok, rozbalte uzel tabulky, vyberte tabulku, filmy. Zadejte obor názvů *modely* a klikněte na **Dokončit** tlačítko.


[![Vytváření pro třídy SQL LINQ](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Obrázek 01**: vytváření třídy LINQ to SQL ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image2.png))


Po dokončení průvodce Entity Data Model Entity Data Model Designer se otevře. Návrhář by měl zobrazit entity filmy (viz obrázek 2).


[![Model dat Entity Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Obrázek 02**: Entity Data Model Designer ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image4.png))


Je potřeba jeden změnu provést, abychom mohli pokračovat. Průvodce dat Entity vygeneruje třídu modelu s názvem *filmy* představující filmy databázové tabulky. Protože třída filmy použijeme k reprezentaci konkrétní film, je potřeba změnit název třídy, která má být *film* místo *filmy* (singulární spíše než množném čísle).

Dvakrát klikněte na název třídy na plochu návrháře a změňte název třídy z filmy film. Po provedení této změny, klikněte **Uložit** tlačítko (ikona disketu) ke generování film třídy.

## <a name="create-the-movies-controller"></a>Vytvoření řadiče filmy

Teď, když máme způsob, jak představují našich záznamů databáze, můžeme vytvořit kontroler, který vrátí kolekce filmy. V okně Průzkumníka řešení Visual Studio klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, řadiče** (viz obrázek 3).


[![Řadiči nabídka přidat](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Obrázek 03**: nabídce přidat řadič ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image6.png))


Když **přidat kontroler** otevře se dialogové okno, zadejte název řadiče MovieController (viz obrázek 4). Klikněte **přidat** tlačítko přidáte nový řadič.


[![Dialogové okno Přidat kontroler](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Obrázek 04**: dialogové okno Přidat kontroler ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image8.png))


Potřebujeme upravit akce Index() vystavené řadičem film tak, aby vracel sadu záznamů databáze. Upravte kontroleru, aby vypadal jako řadič v výpis 1.

**Výpis 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Třída MoviesDBEntities je v výpis 1, používá k reprezentování MoviesDB databáze. Výraz *entity. MovieSet.ToList()* vrací sadu všech filmy z filmy databázové tabulky.

## <a name="create-the-view"></a>Vytvoření zobrazení

Nejjednodušší způsob, jak zobrazit také sadu záznamů databáze do tabulky HTML je využít výhod generování uživatelského rozhraní poskytované sadě Visual Studio.

Sestavit aplikaci tak, že vyberete možnost nabídky **sestavení, sestavit řešení**. Musíte sestavit aplikaci před otevřením **přidat zobrazení** dialogového nebo datových tříd se nezobrazí v dialogovém okně.

Klikněte pravým tlačítkem na Index() akce a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 5).


[![Přidání zobrazení](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Obrázek 05**: Přidání zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image10.png))


V **přidat zobrazení** dialogové okno, zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu**. Vyberte třídu film, jako **zobrazit třída dat**. Vyberte *seznamu* jako **zobrazit obsah** (viz obrázek 6). Výběr těchto možnosti vygeneruje zobrazení silného typu, který zobrazí seznam filmy.


[![Dialogové okno Přidat zobrazení](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Obrázek 06**: Přidat zobrazení dialogu ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image12.png))


Po kliknutí **přidat** tlačítko zobrazení v výpis 2 je generován automaticky. Toto zobrazení obsahuje je kód potřebný k procházení kolekce filmy a zobrazení ke každé z vlastností přehrávání videa.

**Výpis 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Aplikaci můžete spustit výběrem možnosti nabídky **ladit, spustit ladění** (nebo stiskne klávesu F5). Spuštění aplikace spustí se aplikace Internet Explorer. Pokud přejdete na adresu URL /Movie zobrazí stránku na obrázku 7.


[![Tabulku filmy](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Obrázek 07**: tabulku filmy ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-table-of-database-data-vb/_static/image14.png))


Pokud chcete nic o vzhledu mřížky záznamů databáze na obrázku 7 můžete jednoduše upravit zobrazení indexu. Například můžete změnit *DateReleased* hlavičky k *datum vydání* úpravou zobrazení indexu.

## <a name="create-a-template-with-a-partial"></a>Vytvoření šablony se částečné

Při zobrazení získá příliš složitý, je vhodné spustit rozdělení zobrazení na částečné. Použití částečné usnadňuje zobrazení pro pochopení a údržbu. Vytvoříme částečné, můžeme použít jako šablonu pro každý záznam film databáze.

Postupujte podle těchto kroků můžete vytvořit s částečným:

1. Klikněte pravým tlačítkem na složku Views\Movie a vyberte možnost nabídky **přidat zobrazení**.
2. Zaškrtněte políčko s názvem bez přípony *vytvořit částečné zobrazení (.ascx)*.
3. Název s částečným *MovieTemplate*.
4. Zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu**.
5. Vyberte video jako *zobrazit třída dat*.
6. Vyberte prázdný jako *zobrazit obsah*.
7. Klikněte **přidat** tlačítko s částečným přidáte do projektu.

Po dokončení těchto kroků upravte částečné MovieTemplate, aby vypadala jako výpis 3.

**Výpis 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Partial v výpis 3 obsahuje šablonu pro jeden řádek záznamů.

Upravené zobrazení indexu v výpis 4 používá částečné MovieTemplate.

**Výpis 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

Zobrazení v výpis 4 obsahuje pro každou smyčku, který iteruje v rámci všech filmy. Pro každý film částečné MovieTemplate slouží k formátování videa. MovieTemplate je vykreslen metodou voláním metody helper RenderPartial().

Upravené zobrazení indexu vykreslí velmi stejné tabulky HTML záznamů databáze. Však zobrazení je výrazně jednodušší.


Metoda RenderPartial() je jiný než většina jiných metod helper, protože nevrací řetězec. Proto musí volat pomocí metody RenderPartial() &lt;Html.RenderPartial() %&gt; místo &lt;% = Html.RenderPartial() %&gt;.


## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo vysvětlují, jak můžete zobrazit také sadu záznamů databáze do tabulky HTML. Nejprve jste zjistili, jak vracet sadu záznamů databáze z akce kontroleru a využívají k Microsoft Entity Framework. V dalším kroku jste zjistili, jak generovat zobrazení, které zobrazuje kolekce položek automaticky pomocí generování uživatelského rozhraní sady Visual Studio. Nakonec jste zjistili, jak můžete zjednodušit využitím částečné zobrazení. Jste zjistili, jak používat částečné jako šablona, tak, že každý záznam v databázi.

>[!div class="step-by-step"]
[Předchozí](creating-model-classes-with-linq-to-sql-vb.md)
[další](performing-simple-validation-vb.md)
