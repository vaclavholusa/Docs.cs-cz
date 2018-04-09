---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Vytvoření třídy modelu pomocí technologie LINQ to SQL (C#) | Microsoft Docs
author: microsoft
description: Cílem tohoto kurzu je vysvětlit, jednu z metod vytváření tříd modelu pro aplikace ASP.NET MVC. V tomto kurzu zjistíte, jak vytvářet c modelu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f30ff6c97e565059c57c55f26d2dff477774aeb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Vytvoření třídy modelu pomocí technologie LINQ to SQL (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Cílem tohoto kurzu je vysvětlit, jednu z metod vytváření tříd modelu pro aplikace ASP.NET MVC. V tomto kurzu zjistěte, jak vytvářet třídy modelu a provádí přístup k databázi a využívají k Microsoft LINQ to SQL.


Cílem tohoto kurzu je vysvětlit, jednu z metod vytváření tříd modelu pro aplikace ASP.NET MVC. V tomto kurzu zjistíte, jak vytvářet třídy modelu a provádí přístup k databázi a využívají k Microsoft LINQ to SQL

V tomto kurzu jsme vytvořit základní aplikaci film databáze. Jsme začněte vytvořením databázovou aplikaci film nejrychlejší a Nejsnadnějším způsobem možné. Můžeme provádět všechny naše přístup k datům přímo z našich akce kontroleru.

V dalším kroku Další informace o použití vzoru úložiště. Použití vzoru úložiště vyžaduje trochu další práci. Výhodou tohoto vzoru přijetí je však umožňuje vytvářet aplikace, které jsou přizpůsobitelné na změnit a lze je snadno testovat.

## <a name="what-is-a-model-class"></a>Co je třídu modelu?

MVC model obsahuje všechny aplikace logiky, která není obsažen v zobrazení MVC nebo MVC jsou řadič MVC. Konkrétně MVC model obsahuje všechny firemní aplikace a data přístup logiku.

Celou řadu různých technologií můžete použít k implementaci logika data access. Například můžete vytvořit vaše data třídy přístupu pomocí třídy Microsoft Entity Framework, NHibernate, Subsonic nebo ADO.NET.

V tomto kurzu používám technologie LINQ to SQL pro dotazování a aktualizaci databáze. Technologie LINQ to SQL poskytuje metodu s velmi snadno interakci s databázi systému Microsoft SQL Server. Je ale důležité si uvědomit, že není žádným způsobem technologie LINQ to SQL vázaný rozhraní ASP.NET MVC. ASP.NET MVC je kompatibilní s technologií pro přístup k žádné data.

## <a name="create-a-movie-database"></a>Vytvoření databáze filmu

V tomto kurzu – aby ilustrují, jak můžete vytvořit třídy modelu – jsme vytvořit jednoduchou aplikaci film databáze. Prvním krokem je vytvoření nové databáze. Klikněte pravým tlačítkem na aplikaci\_složky dat v okna Průzkumníka řešení a vyberte možnost nabídky **přidat, nové položky**. Vyberte **databáze systému SQL Server** šablony, poskytněte název MoviesDB.mdf a klikněte na **přidat** tlačítko (viz obrázek 1).


[![Přidání nové databáze SQL serveru](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Obrázek 01**: přidávání nové databáze serveru SQL ([Kliknutím zobrazit obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Po vytvoření nové databáze, můžete otevřít databázi dvojitým kliknutím na soubor MoviesDB.mdf v aplikaci\_složku Data. Dvojitým kliknutím na soubor MoviesDB.mdf otevře okno Průzkumníka serveru (viz obrázek 2).

Okno Průzkumníka serveru nazývá okno Průzkumník databáze při použití Visual Web Developer.


[![Používání okna Průzkumníka serveru](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Obrázek 02**: použití okna Průzkumníka serveru ([Kliknutím zobrazit obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Je potřeba přidat do našich databáze, která představuje naše filmy jedna tabulka. Klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**. Výběrem této možnosti nabídky otevře návrháře tabulky (viz obrázek 3).


[![Používání okna Průzkumníka serveru](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Obrázek 03**: Návrhář tabulky ([Kliknutím zobrazit obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Je potřeba přidat následující sloupce do tabulky naše databáze:

| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | celá čísla | False |
| Název | Nvarchar(200) | False |
| Adresář nacházející | Nvarchar(50) | False |

Budete muset udělat dvě věci speciální pro Id sloupce. Nejprve budete muset označte Id sloupec jako sloupec primárního klíče výběrem sloupce v Návrháři tabulky a kliknutím na ikonu klíče. Technologie LINQ to SQL vyžaduje, abyste při provádění vloží nebo aktualizuje v databázi zadat sloupců primárních klíčů.

Dále je nutné označit Id sloupec jako sloupec Identity přiřazením hodnotu Ano **je Identity** vlastnost (viz obrázek 3). Sloupec Identity je sloupec, který je přiřazen nové číslo automaticky při každém přidání nového řádku dat do tabulky.

## <a name="create-linq-to-sql-classes"></a>Vytvoření třídy LINQ to SQL

Naše modelu MVC bude obsahovat LINQ na SQL třídy, které představují tblMovie databázové tabulky. Nejjednodušší způsob, jak vytvořit tyto třídy LINQ to SQL je klikněte pravým tlačítkem na složku modely, vyberte **přidat, nové položky**, vyberte LINQ to SQL třídy šablony, dejte třídy název Movie.dbml a klikněte **přidat**tlačítko (viz obrázek 4).


[![Vytváření pro třídy SQL LINQ](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Obrázek 04**: vytváření třídy LINQ to SQL ([Kliknutím zobrazit obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Ihned po vytvoření film třídy LINQ to SQL, zobrazí se Návrhář relací objektů. Databázové tabulky můžete přetáhnout z okna Průzkumníka serveru na Návrhář relací objektů pro vytvoření třídy LINQ to SQL představující konkrétní databázových tabulek. Je potřeba přidat do Návrhář relací objektů tblMovie databázové tabulky (viz obrázek 5).


[![Pomocí Návrhář relací objektů](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Obrázek 05**: pomocí Návrhář relací objektů ([Kliknutím zobrazit obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Ve výchozím nastavení vytvoří Návrhář relací objektů třídy s velmi stejný název jako tabulku databáze, která přetažením na návrháře. Neradi však volat Naše třída `tblMovie`. Proto klikněte na název třídy v návrháři a změňte název třídy na film.

Nakonec nezapomeňte klikněte **Uložit** tlačítko (obrázek disketová) pro uložení LINQ na třídy SQL. Technologie LINQ to SQL třídy, jinak nebude lze generovat Návrhář relací objektů.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Použití technologie LINQ to SQL v akce Kontroleru

Teď, když máme naše třídy LINQ to SQL, jsme můžete použít tyto třídy k načtení dat z databáze. V této části zjistěte, jak pomocí LINQ SQL tříd přímo v rámci akce kontroleru. Zobrazuje seznam filmy z tabulky databáze tblMovies se budete v zobrazení MVC.

Nejprve musíme upravit HomeController třídy. Tato třída naleznete ve složce řadiče vaší aplikace. Upravte třídy, takže to vypadá třídy ve výpisu 1.

**Výpis 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()` Akce v výpis 1 používá LINQ to SQL DataContext – třída ( `MovieDataContext`) představují `MoviesDB` databáze. `MoveDataContext` Třída vygenerovalo Visual Studio Návrhář relací objektů.

Dotaz LINQ je adresovat DataContext načíst všechny filmy z `tblMovies` databázové tabulky. Seznam filmy je přiřazený k místní proměnné s názvem `movies`. Nakonec seznam filmy předána do zobrazení dat zobrazení.

Chcete-li zobrazit filmy, že potřebujeme další ke změně zobrazení indexu. Můžete najít v zobrazení indexu `Views\Home\` složky. Aktualizace zobrazení Index, aby vypadal podobně jako zobrazení v výpis 2.

**Výpis 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Všimněte si, že se změny zobrazení indexu zahrnuje `<%@ import namespace %>` direktivy v horní části zobrazení. Tato direktiva importuje `MvcApplication1.Models namespace`. Za účelem práce s potřebujeme tento obor názvů `model` třídy – konkrétně `Movie` třída – v zobrazení.

Obsahuje zobrazení v výpis 2 `foreach` smyčky, který iteruje všechny položky reprezentována `ViewData.Model` vlastnost. Hodnota `Title` vlastnost je zobrazena pro každou `movie`.

Všimněte si, že hodnota `ViewData.Model` vlastnost přetypovat `IEnumerable`. To je nezbytné, aby procházení obsahu `ViewData.Model`. Zde Další možností je vytvořit silného typu `view`. Při vytváření silného typu `view`, můžete převést `ViewData.Model` vlastnost pro konkrétní typ ve třídě kódu zobrazení.

Pokud spouštíte aplikaci po úpravě `HomeController` třídy a Index zobrazení pak budete mít prázdnou stránku. Získáte prázdnou stránku, protože nejsou k dispozici žádné záznamy film v `tblMovies` databázové tabulky.

Chcete-li přidat záznamy `tblMovies` tabulky databáze, klikněte pravým tlačítkem myši `tblMovies` databáze tabulkou v okně Průzkumníka serveru (okno Průzkumník databáze v aplikaci Visual Web Developer) a vyberte možnost nabídky Zobrazit Data tabulky. Můžete vložit `movie` záznamů pomocí mřížky, který se zobrazí (viz obrázek 6).


[![Vkládání filmy](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Obrázek 06**: vkládání filmy ([Kliknutím zobrazit obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Po přidání některé záznamy databáze `tblMovies` tabulky a spusťte aplikaci, se zobrazí stránka na obrázku 7. Všechny záznamy databáze film se zobrazí v seznamu s odrážkami.


[![Zobrazení filmy s zobrazení indexu](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Obrázek 07**: zobrazením filmů s zobrazení indexu ([Kliknutím zobrazit obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Použití vzoru úložiště

V předchozí části jsme použili LINQ SQL tříd přímo v rámci akce kontroleru. Použili jsme `MovieDataContex` t třídy přímo z `Index()` akce kontroleru. Není co se v takovém případě jednoduchou aplikaci. Práce přímo se technologie LINQ to SQL v třídě controller však vytvoří problémy, když potřebujete vytvořit složitější aplikaci.

Pomocí technologie LINQ to SQL v rámci třídy kontroleru, je obtížné přepínače technologie pro přístup k datům v budoucnu. Například můžete se rozhodnout přepínat pomocí Microsoft LINQ to SQL k použití jako data přístup technologie Microsoft Entity Framework. V takovém případě bude muset přepisování každý řadič, který přistupuje k databázi v rámci vaší aplikace.

Pomocí technologie LINQ to SQL v rámci třídy kontroleru také ztěžuje sestavení testování částí pro vaši aplikaci. Za normálních okolností nechcete komunikovat s databází při provádění testů jednotek. Chcete použít testů jednotek pro testování vaší aplikace logiky a není databázový server.

Chcete-li sestavit aplikaci MVC, která je více přizpůsobitelné pro budoucí změny a který lze snadno testovat, měli byste zvážit použití vzoru úložiště. Když použijete vzoru úložiště, můžete vytvořit samostatné úložiště třídu, která obsahuje všechna logika přístup k databázi.

Když vytvoříte třídu úložiště, můžete vytvořit rozhraní, které představuje všechny metody použité v třídě úložiště. V rámci řadičů zadejte kód, proti rozhraní místo úložiště. Tímto způsobem můžete implementovat úložiště pomocí technologie pro přístup k jiné data v budoucnosti.

Název rozhraní v výpis 3 `IMovieRepository` a představuje jednu metodu s názvem `ListAll()`.

**Výpis 3 – `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

Implementuje třídu úložiště v výpis 4 `IMovieRepository` rozhraní. Všimněte si, že obsahuje metodu s názvem `ListAll()` odpovídající metodu vyžaduje `IMovieRepository` rozhraní.

**Výpis 4 – `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Nakonec `MoviesController` třídy v výpis 5 využívá schéma úložiště. Už používá LINQ na SQL třídy přímo.

**Výpis 5 – `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Všimněte si, že `MoviesController` třída v výpis 5 má dva konstruktory. První konstruktor konstruktor bez parametrů, je volána, když je aplikace spuštěna. Tento konstruktor vytvoří instanci `MovieRepository` třídy a předává je pro druhý konstruktor.

Druhý konstruktor má jeden parametr: `IMovieRepository` parametr. Tento konstruktor jednoduše přiřadí hodnota parametru na úrovni pole s názvem `_repository`.

`MoviesController` Třída je využívat výhod softwaru návrhový vzor, který volá vzoru vkládání závislostí. Konkrétně je používá takzvaný konstruktor vkládání závislostí. Další informace o tomto vzoru načtením v následujícím článku podle Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Všimněte si, že všechny kód `MoviesController` – třída (s výjimkou prvního konstruktor) komunikuje s `IMovieRepository` rozhraní místo skutečnou `MovieRepository` – třída. Kód spolupracuje s abstraktní rozhraní místo konkrétní implementaci rozhraní.

Pokud chcete upravit technologie přístup dat používá aplikace pak můžete jednoduše implementovat `IMovieRepository` rozhraní s třídu, která používá technologii přístup alternativní databáze. Například můžete vytvořit `EntityFrameworkMovieRepository` třídy nebo `SubSonicMovieRepository` třídy. Protože třídy kontroleru je naprogramovaný tak proti rozhraní, můžete předat na nové implementace `IMovieRepository` řadiče třídy a třídy bude pokračovat v práci.

Kromě toho pokud chcete testovat `MoviesController` třídy, pak můžete předat třídy úložiště falešných film k `HomeController`. Můžete implementovat `IMovieRepository` se třídu, která není ve skutečnosti přístup k databázi, ale obsahuje všechny požadované metody `IMovieRepository` rozhraní. Tímto způsobem můžete testování částí `MoviesController` třída bez skutečně přístup k skutečné databázi.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo ukazují, jak můžete vytvořit třídy modelu MVC a využívají k Microsoft LINQ to SQL. Jsme se zaměřili na dva strategie pro zobrazení dat z databáze v aplikaci ASP.NET MVC. Nejdřív vytvořili LINQ pro třídy SQL a používané třídy přímo v rámci akce kontroleru. Pomocí LINQ SQL tříd v rámci kontroleru umožňuje rychle a snadno zobrazení dat z databáze v aplikaci MVC.

V dalším kroku jsme prozkoumali mírně obtížnější, ale výborný více virtuous cestu pro zobrazení dat databáze. Jsme trvalo výhod použitému vzoru a umístit všechny databáze access logiku do třídy samostatné úložiště. V našem řadiče jsme psali všechny naše kódu proti rozhraní místo konkrétní třídy. Výhodou vzoru úložiště je, že umožňuje nám snadno v budoucnu změnit technologie pro přístup k databázi a umožňuje nám snadno testovatelné naše třídy kontroleru.

> [!div class="step-by-step"]
> [Předchozí](creating-model-classes-with-the-entity-framework-cs.md)
> [další](displaying-a-table-of-database-data-cs.md)
