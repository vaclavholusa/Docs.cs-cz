---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: "Přístup k vašemu modelu datům z řadiče | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f323fe37da739d957a609dc7ca4e71a3c3ab475e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Přístup k vašemu modelu datům z řadiče
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data pro film a zobrazí v prohlížeči pomocí šablony zobrazení.

**Sestavení aplikace** potom přejděte k dalšímu kroku.

Klikněte pravým tlačítkem myši *řadiče* složky a vytvořte novou `MoviesController` řadiče. Níže uvedené možnosti nezobrazí, dokud sestavení aplikace. Vyberte následující možnosti:

- Název řadiče: **MoviesController**. (Toto je výchozí hodnota. )
- Šablona: **kontroler MVC s akcemi čtení/zápisu a zobrazeními, s použitím rozhraní Entity Framework**.
- Třída modelu: **Movie (MvcMovie.Models)**.
- Třída kontextu dat: **MovieDBContext (MvcMovie.Models)**.
- Zobrazení: **Razor (CSHTML)**. (Výchozí nastavení.)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Klikněte na tlačítko **přidat**. Visual Studio Express vytvoří následující soubory a složky:

- *MoviesController.cs* souboru v projektu *řadiče* složky.
- A *filmy* složky v projektu *zobrazení* složky.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, a *Index.cshtml* v novém *Views\Movies* složky.

Automaticky vytvoří CRUD ASP.NET MVC 4 (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení pro vás (Automatická tvorba metody akce CRUD a zobrazení se označuje jako generování uživatelského rozhraní). Nyní máte plně funkční webovou aplikaci, která umožňuje vytvářet, seznam, upravit a odstranit film položky.

Spusťte aplikaci a přejděte do `Movies` řadiče připojením */Movies* na adresu URL na panelu Adresa prohlížeče. Vzhledem k tomu, že se aplikace spoléhá na výchozí směrování (definované v *Global.asax* soubor), požadavek prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí `Index` metody akce `Movies` řadiče. Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je efektivně stejný jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`. Výsledkem je prázdný seznam filmy, protože zatím jste nepřidali žádné.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Vytváření film

Vyberte **vytvořit nový** odkaz. Zadejte některé podrobnosti o film a poté klikněte **vytvořit** tlačítko.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Kliknutím **vytvořit** tlačítko způsobí, že formulář odeslat na server, kde film informace se ukládají v databázi. Potom budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video ve výpisu.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Vytvořte několik další film položky. Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.

## <a name="examining-the-generated-code"></a>Zkoumání generovaný kód

Otevřete *Controllers\MoviesController.cs* soubor a zkontrolujte vygenerovaného `Index` metoda. Část řadičem film s `Index` metoda jsou uvedeny níže.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Následující řádek z `MoviesController` třída vytvoří kontext databáze film, jak je popsáno výše. Dotaz, upravovat a odstraňovat filmy, můžete použít film kontext databáze.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Požadavek na `Movies` vrátí všechny položky v `Movies` tabulky databáze film a pak předá výsledky do `Index` zobrazení.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silného typu modely a @model – klíčové slovo

V tomto kurzu jste viděli, jak řadič může předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu. `ViewBag` Je dynamický objekt, který představuje pohodlný způsob pozdní vazbou předávat informace k zobrazení.

ASP.NET MVC obsahuje také možnost předat silně typované data nebo objekty, které chcete zobrazit šablonu. Tento přístup umožňuje lepší kompilaci Kontrola kódu a bohatší IntelliSense v editoru Visual Studio silného typu. Tento postup se používá mechanismus generování uživatelského rozhraní v sadě Visual Studio `MoviesController` třídy a zobrazení šablony při jeho vytvoření metody a zobrazení.

V *Controllers\MoviesController.cs* zkontrolujte vygenerovaného souboru `Details` metoda. Část řadičem film s `Details` metoda jsou uvedeny níže.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Pokud `Movie` nenajde, instanci `Movie` modelu je předán zobrazení podrobností. Zkontrolujte obsah *Views\Movies\Details.cshtml* souboru.

Zahrnutím `@model` příkaz v horní části souboru šablony zobrazení, můžete zadat typ objektu, která očekává zobrazení. Pokud jste vytvořili řadičem film, Visual Studio automaticky zahrnuty následující `@model` příkaz v horní části *Details.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

To `@model` – direktiva umožňuje přístup k video, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu. Například v *Details.cshtml* šablony, kód předá pro každé pole film `DisplayNameFor` a [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocné objekty HTML s silného typu `Model` objektu. Metody vytvoření a úprava a zobrazení šablony předat také film objektu modelu.

Zkontrolujte *Index.cshtml* zobrazit šablonu a `Index` metoda v *MoviesController.cs* souboru. Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektu při volání `View` Pomocná metoda v `Index` metody akce. Tento kód pak předá `Movies` seznamu z řadiče zobrazení:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Pokud jste vytvořili řadičem film, Visual Studio Express automaticky zahrnuty následující `@model` příkaz v horní části *Index.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

To `@model` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu. Například v *Index.cshtml* šablony, kód prochází filmy nástrojem `foreach` příkaz přes silného typu `Model` objektu:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Protože `Model` je silného typu objektu (jako `IEnumerable<Movie>` objektu), každý `item` objekt ve smyčce je zadán jako `Movie`. Mezi další výhody to znamená, že získat kontrola kompilace kódu a úplné podporu technologie IntelliSense v editoru kódu:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Práce s LocalDB serveru SQL

Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla poskytnuta ukazuje `Movies` databáze, který nebyl ještě neexistuje, Code First vytvoří databázi automaticky. Můžete ověřit, že je vytvořeno podle *aplikace\_Data* složky. Pokud nevidíte *Movies.mdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítka na **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a potom rozbalte *aplikace\_Data* složky.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Klikněte dvakrát na *Movies.mdf* otevřete **PRŮZKUMNÍK databáze**, pak rozbalte **tabulky** složku pro najdete v tabulce filmy.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Pokud se nezobrazí Průzkumník databáze, z **nástroje** nabídce vyberte možnost **připojit k databázi**, zrušení **zvolit zdroj dat** dialogové okno. Tato akce vynutí otevřete Průzkumník databáze.


> [!NOTE]
> Pokud používáte VWD nebo Visual Studio 2010 a dojde k chybě podobná následující následující:
> 
> - Databáze se C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF, nelze otevřít, protože je verze 706. Tento server podporuje verzi 655 a starší. Přechod na starší verzi cesta není podporována.
> - &quot;Pomocí uživatelského kódu se neošetřená výjimka InvalidOperation&quot; v zadaném objektu SqlConnection není určený počáteční katalog.
> 
> Je potřeba nainstalovat [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) a [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Ověřte `MovieDBContext` připojovacího řetězce zadaného na předchozí stránce.


Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **zobrazit Data tabulky** chcete zobrazit data, které jste vytvořili.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **otevřete definici tabulky** zobrazíte v tabulce struktury této Entity Framework Code First automaticky vytvořeny.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Všimněte si jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve. Entity Framework Code First automaticky vytvoří tento schématu na základě vaší `Movie` třídy.

Až budete hotovi, zavřete připojení kliknutím pravým tlačítkem *MovieDBContext* a výběrem **zavřít připojení**. (Pokud nemáte ukončení připojení, může dojde k chybě při příštím spuštění projektu).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Nyní máte databázi a jednoduchý výpis stránku, kterou chcete zobrazit obsah z něj. V dalším kurzu jsme budete zkontrolujte zbytek automaticky generovaný kód a přidejte `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat filmy v této databázi.

>[!div class="step-by-step"]
[Předchozí](adding-a-model.md)
[další](examining-the-edit-methods-and-edit-view.md)
