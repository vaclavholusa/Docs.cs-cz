---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Zobrazení tabulky databázových dat (C#) | Dokumentace Microsoftu
author: microsoft
description: V tomto kurzu se můžu ukazují dvě metody zobrazení sady záznamů v databázi. Můžu zobrazit dvě metody formátování sadu záznamů databáze v HTML ta...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 8409df940e0b5276c4f108531423aadeb2545ca3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828176"
---
<a name="displaying-a-table-of-database-data-c"></a>Zobrazení tabulky databázových dat (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> V tomto kurzu se můžu ukazují dvě metody zobrazení sady záznamů v databázi. Můžu zobrazit dvě metody formátování sadu záznamů databáze v tabulku HTML. Nejprve mohu zobrazit, jak lze formátovat záznamů databáze přímo v rámci zobrazení. V dalším kroku můžu ukazují, jak můžete využít výhod částečných zobrazení při formátování záznamy v databázi.


Cílem tohoto kurzu je vysvětlují, jak můžete zobrazit tabulku HTML databázových dat v aplikaci ASP.NET MVC. Nejprve se dozvíte, jak použít nástroje pro generování uživatelského rozhraní zahrnuté v sadě Visual Studio ke generování zobrazení, které se automaticky zobrazí sadu záznamů. V dalším kroku se dozvíte, jak použít částečné jako šablonu při formátování záznamy v databázi.

## <a name="create-the-model-classes"></a>Vytvoření tříd modelu

Budeme zobrazit sadu záznamů v tabulce databáze filmů. V tabulce databáze filmů obsahuje následující sloupce:

<a id="0.3_table01"></a>


| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | int | False |
| Název | Nvarchar(200) | False |
| Ředitel | NVarchar(50) | False |
| DateReleased | DateTime | False |


Aby mohl představovat filmy tabulky v naší aplikaci ASP.NET MVC, musíme vytvořit třídu modelu. V tomto kurzu vytvoříme pomocí Microsoft Entity Framework našich tříd modelu.

> [!NOTE] 
> 
> V tomto kurzu používáme Microsoft Entity Framework. Je důležité pochopit, že můžete použít celou řadu různých technologií pro interakci s databází z aplikace ASP.NET MVC včetně technologie LINQ to SQL nebo NHibernate, ADO.NET.


Použijte následující postup spuštění Průvodce datovým modelem Entity:

1. Klikněte pravým tlačítkem na složku modely v okně Průzkumník řešení a vyberte možnost nabídky **přidat, nová položka**.
2. Vyberte **Data** kategorie a vyberte **datový Model Entity ADO.NET** šablony.
3. Zadejte název datového modelu *MoviesDBModel.edmx* a klikněte na tlačítko **přidat** tlačítko.

Po kliknutí na tlačítko Přidat, zobrazí se Průvodce datovým modelem Entity (viz obrázek 1). Postupujte podle následujících kroků dokončete průvodce:

1. V **výběr obsahu modelu** kroku, vyberte **Generovat z databáze** možnost.
2. V **vyberte datové připojení** kroku, použijte *MoviesDB.mdf* datové připojení a názvu *MoviesDBEntities* pro nastavení připojení. Klikněte na tlačítko **Další** tlačítko.
3. V **zvolte vaše databázové objekty** krok, rozbalte uzel tabulky, vyberte v tabulce videa. Zadejte obor názvů *modely* a klikněte na tlačítko **Dokončit** tlačítko.


[![Vytvoření LINQ na třídy SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Obrázek 01**: vytváření třídy LINQ to SQL ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image2.png))


Po dokončení Průvodce datovým modelem Entity, otevře se Návrhář Entity Data Model. Návrhář zobrazeno filmy entity (viz obrázek 2).


[![Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Obrázek 02**: Entity Data Model Designer ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image4.png))


Potřebujeme, aby jednu změnu, abychom mohli pokračovat. Průvodce Entity Data vygeneruje třídu modelu s názvem *filmy* , který představuje tabulku databáze filmů. Vzhledem k tomu použijeme filmy třídy představující konkrétní videa, potřeba změnit název třídy, která má být *film* místo *filmy* (singulární spíše než množné číslo).

Dvakrát klikněte na název třídy na návrhové ploše a změňte název třídy z videa na video. Po provedení této změny, klikněte na tlačítko **Uložit** tlačítko (ikona disketu) ke generování třídy Video.

## <a name="create-the-movies-controller"></a>Vytvoří kontroler filmy

Teď, když jsme způsob, jak reprezentaci našich záznamů databáze, můžeme vytvořit kontroler, který vrátí kolekce filmů. V okně Průzkumník řešení Visual Studio klikněte pravým tlačítkem na složku řadiče a vyberte možnost nabídky **přidat, řadič** (viz obrázek 3).


[![Přidání Kontroleru nabídky](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Obrázek 03**: The přidání Kontroleru nabídky ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image6.png))


Když **přidat kontroler** se zobrazí dialogové okno, zadejte název řadiče MovieController (viz obrázek 4). Klikněte na tlačítko **přidat** tlačítko pro přidání nového řadiče.


[![Dialogové okno Přidat kontroler](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Obrázek 04**: dialogové okno pro přidání Kontroleru ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image8.png))


Musíme akce Index() vystavené film řadič tak, aby vracel sadu záznamů databáze změnit. Upravte kontrolér tak, aby vypadal jako řadič v informacích 1.

**Výpis 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

Ve výpisu 1 je třída MoviesDBEntities používá k reprezentování MoviesDB databáze. Pro tuto třídu použít, budete muset importovat MvcApplication1.Models oboru názvů následujícím způsobem:

pomocí MvcApplication1.Models;

Výraz *entity. MovieSet.ToList()* vrací sadu všech filmy z databázové tabulky videa.

## <a name="create-the-view"></a>Vytvoření zobrazení

Nejjednodušší způsob, jak zobrazit sadu záznamů databáze v tabulce HTML je využívat generování uživatelského rozhraní poskytovaný sadou Visual Studio.

Sestavení aplikace tak, že vyberete možnost nabídky **vytvořit, sestavit řešení**. Je nutné vytvořit aplikaci před otevřením **přidat zobrazení** nebude v dialogovém okně zobrazí dialogové okno nebo datových tříd.

Klikněte pravým tlačítkem na akce Index() a vyberte možnost nabídky **přidat zobrazení** (viz obrázek 5).


[![Přidání zobrazení](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Obrázek 05**: Přidání zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image10.png))


V **přidat zobrazení** dialogového okna, zaškrtněte políčko s popiskem **vytvoření zobrazení se silnými typy**. Vyberte třídu film, jako **zobrazení dat třídy**. Vyberte *seznamu* jako **zobrazit obsah** (viz obrázek 6). Výběr tyto možnosti budou generovat zobrazení silného typu, který zobrazí seznam filmy.


[![Dialogové okno Přidat zobrazení](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Obrázek 06**: dialogové okno pro přidání zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image12.png))


Po klepnutí **přidat** automaticky generováno tlačítko, zobrazení, ve výpisu 2. Toto zobrazení obsahuje kód potřebný k iteraci v rámci kolekce filmů a zobrazit vlastnosti videa.

**Výpis 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Aplikaci můžete spustit tak, že vyberete možnost nabídky **ladit, spustit ladění** (nebo stisknutí klávesy F5). Spuštění aplikace se spustí aplikace Internet Explorer. Když přejdete na adresu URL /Movie uvidíte stránku na obrázku 7.


[![Tabulku filmy](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Obrázek 07**: tabulku filmy ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-table-of-database-data-cs/_static/image14.png))


Pokud se vám nic o vzhledu mřížky databázových záznamů na obrázku 7 můžete jednoduše upravit zobrazení indexu. Například můžete změnit *DateReleased* záhlaví *datum vydání* úpravou zobrazení indexu.

## <a name="create-a-template-with-a-partial"></a>Vytvoření šablony s částečné

Při zobrazení získá příliš složitý, je vhodné spustit rozdělení do částečné zobrazení. Pomocí částečných zobrazení usnadňuje zobrazení pochopit a udržovat. Vytvoříme částečné, můžeme použít jako šablonu k formátování jednotlivých záznamů databáze filmů.

Postupujte podle těchto kroků můžete vytvořit části:

1. Klikněte pravým tlačítkem na složku Views\Movie a vyberte možnost nabídky **přidat zobrazení**.
2. Zaškrtněte políčko s popiskem *vytvořit částečné zobrazení (prvků.ascx)*.
3. Pojmenujte částečnou *MovieTemplate*.
4. Zaškrtněte políčko s popiskem **vytvoření zobrazení se silnými typy**.
5. Vyberte video jako *zobrazení dat třídy*.
6. Vyberte prázdný jako *zobrazit obsah*.
7. Klikněte na tlačítko **přidat** tlačítko pro přidání částečné do projektu.

Po dokončení těchto kroků upravte MovieTemplate částečné vypadat výpis 3.

**Výpis 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Částečné v 3 výpis obsahuje šablony pro jeden řádek záznamů.

Upravené zobrazení indexu v informacích 4 používá MovieTemplate částečné.

**Část 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

Zobrazení v informacích 4 obsahuje smyčku foreach, která iteruje přes všechny videa. Pro každý film částečné MovieTemplate slouží k formátování videa. MovieTemplate generován voláním metody helper RenderPartial().

Upravené Index zobrazení vykreslí velmi stejné tabulky HTML záznamů databáze. Ale zobrazení je výrazně zjednodušené.


Metoda RenderPartial() je jiná než většina jiných metod helper, protože nevrací řetězec. Proto musí volat metoda RenderPartial() použití &lt;% Html.RenderPartial(); %&gt; místo &lt;% = Html.RenderPartial(); %&gt;.


## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo vysvětlují, jak můžete zobrazit sadu záznamů databáze v tabulku HTML. Nejdřív jste zjistili, jak vrátit sadu záznamů databáze z akce kontroleru s využitím rozhraní Entity Framework společnosti Microsoft. Dále jste zjistili, jak používat generování uživatelského rozhraní sady Visual Studio vygenerovat zobrazení, která zobrazuje kolekce položek automaticky. Nakonec jste zjistili, jak zjednodušit využití výhod částečné zobrazení. Naučili jste se použít částečné jako šablonu tak, aby každý záznam v databázi lze formátovat.

> [!div class="step-by-step"]
> [Předchozí](creating-model-classes-with-linq-to-sql-cs.md)
> [další](performing-simple-validation-cs.md)
