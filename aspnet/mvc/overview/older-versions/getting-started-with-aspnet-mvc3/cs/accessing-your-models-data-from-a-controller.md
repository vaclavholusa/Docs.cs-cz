---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: "Přístup k vašemu modelu datům z řadiče (C#) | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5ee29dbc5b4566273592041d94458104e6e0f65e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Přístup k vašemu modelu datům z řadiče (C#)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu. [Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.

V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data pro film a zobrazí v prohlížeči pomocí šablony zobrazení. Ujistěte se, že vytvoříte svou aplikaci, než budete pokračovat.

Klikněte pravým tlačítkem myši *řadiče* složky a vytvořte novou `MoviesController` řadiče. Vyberte následující možnosti:

- Název řadiče: **MoviesController**. (Toto je výchozí hodnota. )
- Šablona: **řadiče s akcemi čtení/zápisu a zobrazeními, používá nástroj Entity Framework**.
- Třída modelu: **Movie (MvcMovie.Models)**.
- Třída kontextu dat: **MovieDBContext (MvcMovie.Models)**.
- Zobrazení: **Razor (CSHTML)**. (Výchozí nastavení.)

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Klikněte na tlačítko **přidat**. Visual Web Developer vytvoří následující soubory a složky:

- *MoviesController.cs* souboru v projektu *řadiče* složky.
- A *filmy* složky v projektu *zobrazení* složky.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, a *Index.cshtml* v novém *Views\Movies* složky.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

Tento mechanismus generování uživatelského rozhraní ASP.NET MVC 3 automaticky vytvoří CRUD (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení pro vás. Nyní máte plně funkční webovou aplikaci, která umožňuje vytvářet, seznam, upravit a odstranit film položky.

Spusťte aplikaci a přejděte do `Movies` řadiče připojením */Movies* na adresu URL na panelu Adresa prohlížeče. Vzhledem k tomu, že se aplikace spoléhá na výchozí směrování (definované v *Global.asax* soubor), požadavek prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí `Index` metody akce `Movies` řadiče. Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je efektivně stejný jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`. Výsledkem je prázdný seznam filmy, protože zatím jste nepřidali žádné.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Vytváření film

Vyberte **vytvořit nový** odkaz. Zadejte některé podrobnosti o film a poté klikněte **vytvořit** tlačítko.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Kliknutím **vytvořit** tlačítko způsobí, že formulář odeslat na server, kde film informace se ukládají v databázi. Potom budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video ve výpisu.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Vytvořte několik další film položky. Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.

## <a name="examining-the-generated-code"></a>Zkoumání generovaný kód

Otevřete *Controllers\MoviesController.cs* soubor a zkontrolujte vygenerovaného `Index` metoda. Část řadičem film s `Index` metoda jsou uvedeny níže.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Následující řádek z `MoviesController` třída vytvoří kontext databáze film, jak je popsáno výše. Dotaz, upravovat a odstraňovat filmy, můžete použít film kontext databáze.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Požadavek na `Movies` vrátí všechny položky v `Movies` tabulky databáze film a pak předá výsledky do `Index` zobrazení.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silného typu modely a @model – klíčové slovo

V tomto kurzu jste viděli, jak řadič může předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu. `ViewBag` Je dynamický objekt, který představuje pohodlný způsob pozdní vazbou předávat informace k zobrazení.

ASP.NET MVC obsahuje také možnost předat silně typované data nebo objekty, které chcete zobrazit šablonu. Tento přístup umožňuje lepší kompilaci Kontrola kódu a bohatší IntelliSense v editoru Visual Web Developer silného typu. Používáme tento přístup s `MoviesController` třídy a *Index.cshtml* zobrazit šablonu.

Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektu při volání `View` Pomocná metoda v `Index` metody akce. Tento kód pak předá `Movies` seznamu z řadiče zobrazení:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Zahrnutím `@model` příkaz v horní části souboru šablony zobrazení, můžete zadat typ objektu, která očekává zobrazení. Pokud jste vytvořili řadičem film, Visual Web Developer automaticky zahrnuty následující `@model` příkaz v horní části *Index.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

To `@model` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu. Například v *Index.cshtml* šablony, kód prochází filmy nástrojem `foreach` příkaz přes silného typu `Model` objektu:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Protože `Model` je silného typu objektu (jako `IEnumerable<Movie>` objektu), každý `item` objekt ve smyčce je zadán jako `Movie`. Mezi další výhody to znamená, že získat kontrola kompilace kódu a úplné podporu technologie IntelliSense v editoru kódu:

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Práce s SQL Server Compact

Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla poskytnuta ukazuje `Movies` databáze, který nebyl ještě neexistuje, Code First vytvoří databázi automaticky. Můžete ověřit, že je vytvořeno podle *aplikace\_Data* složky. Pokud nevidíte *Movies.sdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítka na **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a potom rozbalte *aplikace\_Data* složky.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Klikněte dvakrát na *Movies.sdf* otevřete **Průzkumníka serveru**. Potom rozbalte **tabulky** složky zobrazíte tabulky, které byly vytvořeny v databázi.

> [!NOTE]
> Pokud dojde k chybě při poklepání *Movies.sdf*, ujistěte se, že jste nainstalovali [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + nástroje pro podporu). (Odkazy na softwaru, najdete v seznamu požadavků v část 1 / Tento kurz řady.) Pokud ve verzi nainstalovat nyní, budete muset zavřete a znovu otevřete Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Existují dvě tabulky, jeden pro `Movie` sady entit a potom `EdmMetadata` tabulky. `EdmMetadata` Tabulku rozhraní Entity Framework použije k určení, kdy modelu a databáze nejsou synchronizovány.

Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **zobrazit Data tabulky** chcete zobrazit data, které jste vytvořili.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **upravit schéma tabulky**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Všimněte si jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve. Entity Framework Code First automaticky vytvoří tento schématu na základě vaší `Movie` třídy.

Až budete hotovi, zavřete připojení. (Pokud nemáte ukončení připojení, může dojde k chybě při příštím spuštění projektu).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Nyní máte databázi a jednoduchý výpis stránku, kterou chcete zobrazit obsah z něj. V dalším kurzu jsme budete zkontrolujte zbytek automaticky generovaný kód a přidejte `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat filmy v této databázi.

>[!div class="step-by-step"]
[Předchozí](adding-a-model.md)
[další](examining-the-edit-methods-and-edit-view.md)
