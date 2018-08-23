---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Přístup k datům modelu z Kontroleru (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 526f3b532a0b5c1c5c6d0cf8a9e17899300ca237
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752043"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>Přístup k datům modelu z Kontroleru (VB)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu. [Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/accessing-your-models-data-from-a-controller.md) tohoto kurzu.


V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data o filmech a zobrazí v prohlížeči pomocí zobrazení šablony. Ujistěte se, že jste svou aplikaci, než budete pokračovat.

Klikněte pravým tlačítkem myši *řadiče* složky a vytvořte nový `MoviesController` kontroleru. Vyberte následující možnosti:

- Název kontroleru: **MoviesController**. (Toto je výchozí.)
- Šablona: **řadiče s akcemi čtení/zápisu a zobrazeními, používá nástroj Entity Framework**.
- Třída modelu: **Movie (MvcMovie.Models)**.
- Třída kontextu dat: **MovieDBContext (MvcMovie.Models)**.
- Zobrazení: **Razor (CSHTML)**. (Výchozí nastavení.)

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Klikněte na tlačítko **přidat**. Visual Web Developer vytvoří následující soubory a složky:

- *MoviesController.vb* souboru v projektu *řadiče* složky.
- A *filmy* složky v projektu *zobrazení* složky.
- *Create.vbhtml Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, a *Index.vbhtml* na novém *Views\Movies* složky.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Mechanismus generování uživatelského rozhraní ASP.NET MVC 3 automaticky vytvoří CRUD (vytváření, čtení, aktualizace a odstranění) metody akce a zobrazení za vás. Teď máte plně funkční webovou aplikaci, která umožňuje vytvořit, seznam, upravovat a odstraňovat položky video.

Spusťte aplikaci a přejděte `Movies` řadič přidáním */Movies* na adresu URL do adresního řádku prohlížeče. Protože aplikace se spoléhá na výchozí směrování (definované v *Global.asax* souboru), žádost prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí hodnotu `Index` metody akce `Movies` kontroleru. Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je v podstatě totéž jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`. Výsledkem je prázdný seznam filmy, protože ještě jste nepřidali žádné.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Vytváření video

Vyberte **vytvořit nový** odkaz. Zadejte podrobnosti o videa a poté klikněte **vytvořit** tlačítko.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Kliknutím **vytvořit** tlačítko způsobí, že formulář, který se má publikovat na server, kde je video informace uloženy v databázi. Pak budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video v seznamu.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Vytvořte několik další položky video. Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.

## <a name="examining-the-generated-code"></a>Zkoumání generovaného kódu

Otevřít *Controllers\MoviesController.vb* souboru a prozkoumání generované `Index` metody. Část kontroler filmů s `Index` metoda je uveden níže.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

Následující řádek ze `MoviesController` třídy vytvoří kontext databáze filmů, jak je popsáno výše. Kontext databáze filmů slouží k dotazování, upravovat a odstraňovat videa.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Požadavek na `Movies` kontroler vrací všechny položky v `Movies` tabulky movie databáze a potom předává výsledky `Index` zobrazení.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modely se silnými typy a @model – klíčové slovo

Dříve v tomto kurzu jste viděli, jak kontroleru můžete předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu. `ViewBag` Je dynamický objekt, který představuje pohodlný způsob s pozdní vazbou k předávání informací do zobrazení.

ASP.NET MVC rovněž poskytuje možnost předávání silně typované dat nebo objekty, které chcete zobrazit šablonu. Tento přístup umožňuje lepší kompilace Kontrola kódu a propracovanější IntelliSense v editoru Visual Web Developer silného typu. Používáme s tímto přístupem `MoviesController` třídy a *Index.vbhtml* zobrazit šablonu.

Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektu při volání `View` pomocnou metodu v `Index` metody akce. Tento kód pak předá `Movies` seznamu z kontroleru zobrazení:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Zahrnutím `@ModelType` příkazu v horní části souboru šablony zobrazení, můžete určit typ objektu, který očekává, že zobrazení. Při vytvoření kontroleru video Visual Web Developer automaticky zahrnuty následující `@model` příkazu v horní části *Index.vbhtml* souboru:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

To `@ModelType` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předána do zobrazení podle používání `Model` objekt, který je silně typováno. Například v *Index.vbhtml* šablony, kód prochází videa prováděním `foreach` příkaz přes silného typu `Model` objektu:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Protože `Model` objektu je silně typováno (jako `IEnumerable<Movie>` objektu), každý `item` objektu ve smyčce je zadán jako `Movie`. Kromě dalších výhod to znamená získání kompilace Kontrola kódu a plnou podporu technologie IntelliSense v editoru kódu:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Práce s SQL serverem Compact

Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla k dispozici na který je odkazováno `Movies` databázi, která tenkrát neexistovaly, takže Code First vytvořit databáze automaticky. Můžete ověřit, že se vytvořil zobrazením *aplikace\_Data* složky. Pokud se nezobrazí *Movies.sdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítko **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a pak rozbalte *aplikace\_Data* složky.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Dvakrát klikněte na panel *Movies.sdf* otevřete **Průzkumníka serveru**. Pak rozbalte **tabulky** složky tabulky, které byly vytvořeny v databázi.

> [!NOTE]
> Pokud dojde k chybě při poklepání *Movies.sdf*, ujistěte se, že jste nainstalovali **Visual Studio 2010 SP1 nástroje pro SQL Server Compact 4.0**. (Odkazy na software najdete v seznamu požadavků uvedených v části 1 této série kurzů.) Pokud je nyní nainstalovat verzi, budete muset zavřít a znovu otevřete Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Existují dvě tabulky, jeden pro `Movie` sady entit a pak `EdmMetadata` tabulky. `EdmMetadata` Tabulka Entity Framework používá k určení, kdy nejsou synchronizované modelu a databáze.

Klikněte pravým tlačítkem myši `Movies` tabulce a vybrat **zobrazit Data tabulky** k zobrazení dat, které jste vytvořili.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Klikněte pravým tlačítkem myši `Movies` tabulce a vybrat **upravit schéma tabulky**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Všimněte si, že jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve. Entity Framework Code First automaticky vytvoří toto schéma na základě vašich `Movie` třídy.

Jakmile budete hotovi, ukončete připojení. (Pokud není ukončení připojení, pravděpodobně dojde k chybě při příštím spuštění projektu).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Teď máte databázi a jednoduchý seznam stránku k zobrazení obsahu z něj. V dalším kurzu vytvoříme zkontrolujte zbytek automaticky generovaný kód a přidat `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat videa v této databázi.

> [!div class="step-by-step"]
> [Předchozí](adding-a-model.md)
> [další](examining-the-edit-methods-and-edit-view.md)
