---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Vytvoření tříd modelu pomocí LINQ to SQL (C#) | Dokumentace Microsoftu
author: microsoft
description: Cílem tohoto kurzu je vysvětlit jednu z metod vytvoření tříd modelu pro aplikace ASP.NET MVC. V tomto kurzu se dozvíte, jak sestavit model c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: c76e92ebfab1db162151ea5753888a60a4d54e59
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753056"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Vytvoření tříd modelu pomocí LINQ to SQL (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Cílem tohoto kurzu je vysvětlit jednu z metod vytvoření tříd modelu pro aplikace ASP.NET MVC. V tomto kurzu se dozvíte, jak k vytvoření tříd modelu a provádění přístup k databázi s využitím Microsoft LINQ to SQL.


Cílem tohoto kurzu je vysvětlit jednu z metod vytvoření tříd modelu pro aplikace ASP.NET MVC. V tomto kurzu se dozvíte, jak k vytvoření tříd modelu a provádění přístup k databázi s využitím Microsoft LINQ to SQL

V tomto kurzu jsme integrovali základní aplikace Movie database. Začneme vytvořením aplikace Movie database v nejrychlejší a nejjednodušší způsob je to možné. Můžeme provádět všechny naše přístup k datům přímo z našich akce kontroleru.

V dalším kroku se dozvíte, jak použít model úložiště. Použití modelu úložiště vyžaduje trochu více práce. Výhodou přechodu tohoto modelu je však umožňuje vytvářet aplikace, které jsou přizpůsobitelné, na změnit a lze je snadno testovat.

## <a name="what-is-a-model-class"></a>Co je třídu modelu?

MVC model obsahuje všechny aplikační logiky, který není obsažen v zobrazení MVC nebo kontroler MVC. Zejména modelu MVC obsahuje všechny vaše obchodní aplikace a logiky přístupu k datům.

K implementaci logikou přístupu dat můžete použít celou řadu různých technologií. Můžete například vytvořit tříd pro přístup k vaší data pomocí tříd Microsoft Entity Framework, NHibernate, Subsonic nebo ADO.NET.

V tomto kurzu používám LINQ to SQL pro dotazování a aktualizaci databáze. Technologie LINQ to SQL vám poskytne velmi snadné způsob interakce s databází systému Microsoft SQL Server. Je důležité pochopit, že rozhraní ASP.NET MVC se neváže na LINQ to SQL žádným způsobem. ASP.NET MVC je kompatibilní s technologií přístupu všechny data.

## <a name="create-a-movie-database"></a>Vytvoření databáze filmů

V tomto kurzu – k ilustraci, jak se dají vytvářet tříd modelu – jsme sestavení jednoduché aplikace Movie database. Prvním krokem je vytvoření nové databáze. Klikněte pravým tlačítkem na aplikaci\_složce dat v okně Průzkumníka řešení a vyberte možnost nabídky **přidat, nová položka**. Vyberte **databázi systému SQL Server** šablony, zadejte pro něj název MoviesDB.mdf a klikněte na tlačítko **přidat** tlačítko (viz obrázek 1).


[![Přidání nové databáze SQL serveru](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Obrázek 01**: Přidání nové databáze SQL serveru ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Jakmile vytvoříte novou databázi, můžete otevřít databázi dvojitým kliknutím na soubor MoviesDB.mdf v aplikaci\_složku Data. Dvojitým kliknutím na soubor MoviesDB.mdf se otevře okno Průzkumníka serveru (viz obrázek 2).

Okno Průzkumníka serveru je volána v okně Průzkumník databáze při použití aplikace Visual Web Developer.


[![Pomocí Průzkumníka serveru](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Obrázek 02**: použití okna Průzkumníka serveru ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Potřebujeme přidat jedné tabulky do databáze, která představuje naše videa. Klikněte pravým tlačítkem na složku tabulky a vyberte možnost nabídky **přidat novou tabulku**. Tato možnost nabídky vyberete, otevře se Návrhář tabulky (viz obrázek 3).


[![Pomocí Průzkumníka serveru](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Obrázek 03**: The návrháře tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Potřebujeme přidat následující sloupce do naší tabulky databáze:

| **Název sloupce** | **Datový typ** | **Povolit hodnoty Null** |
| --- | --- | --- |
| ID | int | False |
| Název | Nvarchar(200) | False |
| Ředitel | nvarchar(50) | False |

Budete muset udělat dvě věci speciální pro Id sloupce. Nejprve budete muset označit Id sloupec jako sloupec primárního klíče výběrem sloupce v Návrháři tabulek a kliknutím na ikonu klíče. Technologie LINQ to SQL vyžaduje, abyste při provádění vloží nebo aktualizuje databázi zadat sloupců primárního klíče.

Dále je třeba označit Id sloupec jako sloupec Identity přiřazením hodnotu Ano **je identita** vlastnosti (viz obrázek 3). Sloupec Identity je sloupec, který je přiřazen nové číslo automaticky pokaždé, když přidáte nový řádek dat do tabulky.

## <a name="create-linq-to-sql-classes"></a>Vytvoření třídy LINQ to SQL

Náš model MVC bude obsahovat LINQ na třídy SQL, které představují tblMovie databázové tabulky. Klikněte pravým tlačítkem na složku modely, vyberte je nejjednodušší způsob, jak vytvořit tyto třídy LINQ to SQL **přidat, nová položka**, vyberte LINQ na třídy SQL šablonu, zadejte název Movie.dbml třídy a klikněte na **přidat**tlačítko (viz obrázek 4).


[![Vytvoření LINQ na třídy SQL](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Obrázek 04**: vytváření třídy LINQ to SQL ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Ihned po vytvoření film LINQ na třídy SQL, zobrazí se Návrhář relací objektů. Databázové tabulky můžete přetáhnout z okna Průzkumníka serveru do Návrháře relací objektů k vytvoření třídy LINQ to SQL, které představují konkrétní databázové tabulky. Je potřeba přidat tblMovie databázové tabulky do Návrháře relací objektů (viz obrázek 5).


[![Pomocí Návrháře relací objektů](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Obrázek 05**: pomocí Návrháře relací objektů ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Ve výchozím nastavení vytvoří Návrhář relací objektů třídy s velmi stejný název jako tabulku databáze, můžete přetáhnout do návrháře. Nechceme však volat Naše třída `tblMovie`. Proto klikněte na název třídy v návrháři a změnit název třídy na video.

Nakonec se nezapomeňte kliknout na **Uložit** tlačítko (přehled o disketová) Chcete-li uložit LINQ na třídy SQL. V opačném případě nebude vygenerováno LINQ na třídy SQL pomocí Návrháře relací objektů.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Použití technologie LINQ to SQL v akce Kontroleru

Teď, když jsme naše třídy LINQ to SQL, můžeme použít tyto třídy k načtení dat z databáze. V této části se dozvíte, jak používat LINQ na třídy SQL přímo v rámci akce kontroleru. Seznam videa z databázové tabulky tblMovies budete zobrazují v zobrazení MVC.

Nejdřív potřebujeme upravit HomeController třídy. Tato třída najdete ve složce řadiče vaší aplikace. Upravte třídu, takže to vypadá třídy ve výpisu 1.

**Výpis 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()` Akce v informacích 1 používá LINQ na třídy SQL DataContext ( `MovieDataContext`) představující `MoviesDB` databáze. `MoveDataContext` Třída byla vygenerována Visual Studio Návrhář relací objektů.

Dotaz LINQ se provádí proti DataContext načítat všechna videa z `tblMovies` databázové tabulky. Seznam filmy je přiřazen do místní proměnné s názvem `movies`. A konečně seznam filmy předána do zobrazení dat zobrazení.

Chcete-li zobrazit videa, musíme dále upravit zobrazení indexu. Můžete najít v zobrazení pro Index `Views\Home\` složky. Aktualizace zobrazení Index tak, aby vypadal jako zobrazení výpisu 2.

**Výpis 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Všimněte si, že upravené zobrazení indexu zahrnuje `<%@ import namespace %>` direktiv v horní části stránky zobrazení. Tato direktiva importuje `MvcApplication1.Models namespace`. Za účelem práce s musíme tento obor názvů `model` třídy – zejména `Movie` třídy v zobrazení.

Obsahuje zobrazení, ve výpisu 2 `foreach` smyčku, která iteruje přes všechny položky reprezentována `ViewData.Model` vlastnost. Hodnota `Title` vlastnost se zobrazí pro každou `movie`.

Všimněte si, že hodnota `ViewData.Model` vlastnost přetypováno na `IEnumerable`. To je nezbytné, aby bylo možné projít obsah `ViewData.Model`. Tady Další možností je vytvořit silného typu `view`. Při vytváření silného typu `view`, můžete přetypovat `ViewData.Model` na konkrétní typ v třídě modelu code-behind zobrazení.

Pokud spustíte aplikaci po změně `HomeController` třídy a Index zobrazení obdržíte prázdnou stránku. Získáte prázdnou stránku, protože neexistují žádné záznamy filmu v `tblMovies` databázové tabulky.

Chcete-li přidat záznamy `tblMovies` tabulky databáze, klikněte pravým tlačítkem myši `tblMovies` databázové tabulce v okně Průzkumníka serveru (okno Průzkumník databáze v aplikaci Visual Web Developer) a vyberte možnost nabídky Zobrazit Data tabulky. Můžete vložit `movie` záznamů pomocí mřížky, která se zobrazí (viz obrázek 6).


[![Vložení videa](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Obrázek 06**: vložení videa ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Poté, co přidáte některé záznamy databáze `tblMovies` tabulky a spusťte aplikaci, zobrazí se stránka na obrázku 7. Všechny záznamy databáze filmů se zobrazí v seznamu s odrážkami.


[![Zobrazení videa pomocí zobrazení indexu](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Obrázek 07**: zobrazení videa pomocí zobrazení indexu ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Použití modelu úložiště

V předchozí části jsme použili LINQ na třídy SQL přímo v rámci akce kontroleru. Jsme použili `MovieDataContext` třídy přímo `Index()` akce kontroleru. Není nic špatného se to v případě jednoduchou aplikaci. Ale práci přímo s LINQ to SQL ve třídě controller vytvoří problémy, když budete muset vytvořit složitější aplikaci.

Použití technologie LINQ to SQL v rámci třídy kontroleru je obtížné přepnout technologií přístupu k datům v budoucnu. Například můžete rozhodnout přepnou od používání Microsoft LINQ to SQL pomocí Microsoft Entity Framework jako technologie data access. V takovém případě je třeba přepsat každý kontroler, který přistupuje k databázi v rámci vaší aplikace.

V rámci třídy kontroleru pomocí LINQ to SQL také ztěžuje k vytváření testů jednotek pro vaši aplikaci. Za normálních okolností nechcete pracovat s databází při provádění testů jednotek. Chcete použít testů jednotek pro testování vaší aplikace logiky a ne vašemu databázovému serveru.

Aby bylo možné sestavit aplikaci MVC, která je více přizpůsobitelná pro budoucí změny a, který lze snadno testovat, měli byste zvážit použití modelu úložiště. Při použití vzoru úložiště vytvořte oddělené úložiště třídu, která obsahuje všechny logiky přístupu k databázi.

Když vytvoříte třídu úložiště, můžete vytvořit rozhraní, který reprezentuje všechny metody, které používá třída úložiště. V řadiči napište svůj kód proti rozhraní místo úložiště. Tímto způsobem můžete implementovat úložiště pomocí technologií přístupu k datům různých v budoucnu.

Název rozhraní v informacích 3 `IMovieRepository` a představuje jedinou metodu s názvem `ListAll()`.

**Výpis 3 – `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

Implementuje třídu úložiště v informacích 4 `IMovieRepository` rozhraní. Všimněte si, že obsahuje metodu s názvem `ListAll()` odpovídající metodu vyžadované `IMovieRepository` rozhraní.

**Část 4 – `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Nakonec `MoviesController` třída v informacích 5 používá model úložiště. Už používá LINQ na třídy SQL přímo.

**Výpis 5 – `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Všimněte si, že `MoviesController` třída v informacích 5 má dva konstruktory. První konstruktor, konstruktor bez parametrů, je volána, když je aplikace spuštěná. Tento konstruktor vytvoří instanci `MovieRepository` třídy a předává je na druhý konstruktor.

Druhý konstruktor má jeden parametr: `IMovieRepository` parametru. Tento konstruktor jednoduše přiřadí hodnotu parametru na úrovni pole s názvem `_repository`.

`MoviesController` Třídy je s využitím softwaru návrhový vzor, který volá vzor vkládání závislostí. Zejména používá se nazývá konstruktor vkládání závislostí. Další informace o tomto vzoru najdete v následujícím článku od Martina Fowlera:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Všimněte si, že veškerý kód v `MoviesController` třídy (s výjimkou první konstruktor) komunikuje `IMovieRepository` rozhraní namísto skutečné `MovieRepository` třídy. Kód spolupracuje s abstraktní rozhraní místo konkrétní implementaci rozhraní.

Pokud chcete upravit technologií přístupu dat v aplikaci použít, můžete jednoduše implementovat `IMovieRepository` rozhraní s třídou, která používá technologii přístupu alternativní databáze. Například můžete vytvořit `EntityFrameworkMovieRepository` třídy nebo `SubSonicMovieRepository` třídy. Protože třída kontroleru je programovat proti rozhraní, můžete předat novou implementaci `IMovieRepository` ke kontroleru třídy a třídy by pokračovat v práci.

Kromě toho pokud chcete testovat `MoviesController` třídy, pak můžete předat třídu úložiště falešné video `HomeController`. Můžete implementovat `IMovieRepository` třída s atributem třídy, která není ve skutečnosti přístup k databázi, ale obsahuje všechny požadované metody `IMovieRepository` rozhraní. Tímto způsobem můžete testování částí `MoviesController` třídy bez skutečně přístup ke skutečné databázi.

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo ukazují, jak můžete vytvořit třídy modelu MVC s využitím Microsoft LINQ to SQL. Jsme se zaměřili na dvou strategií pro zobrazení dat z databáze v aplikaci ASP.NET MVC. Nejprve jsme vytvořili LINQ na třídy SQL a používané třídy přímo v rámci akce kontroleru. Pomocí jazyka LINQ na třídy SQL v rámci kontroleru umožňuje rychle a snadno zobrazení dat z databáze v aplikaci MVC.

V dalším kroku Prozkoumali jsme o něco složitější, ale jednoznačně více porozumí cestu pro zobrazení dat z databáze. Jsme využil použitému vzoru a umístit všechny naše logikou přístupu k databázi ve třídě oddělené úložiště. V kontroleru jsme napsali všechny našeho kódu proti rozhraní místo konkrétní třídy. Výhodou model úložiště je, že umožňuje nám zvýšit v budoucnu snadno změnit technologie pro přístup k databázi a umožňuje nám zvýšit snadno testujte naší třídy kontroleru.

> [!div class="step-by-step"]
> [Předchozí](creating-model-classes-with-the-entity-framework-cs.md)
> [další](displaying-a-table-of-database-data-cs.md)
