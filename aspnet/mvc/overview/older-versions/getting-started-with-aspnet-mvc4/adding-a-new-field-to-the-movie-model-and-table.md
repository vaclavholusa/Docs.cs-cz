---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: "Přidání nové pole do modelu film a tabulka | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 0c094c4d4c99702a5b513717126872a254ca3e5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Přidání nové pole do modelu film a tabulky
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části používáte migrace Code First Entity Framework pro migraci některé změny do třídy modelu tak, že použití této změny do databáze.

Ve výchozím nastavení při použití Entity Framework Code First automaticky vytvořit databázi, stejně jako dříve v tomto kurzu Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze. Pokud nejsou synchronizované, rozhraní Entity Framework vrátí chybu. To usnadňuje sledovat problémy v čase vývoj, který může jinak pouze pro vás (podle skrytého chyby) v době běhu.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Nastavení migrace Code First pro změny modelu

Pokud používáte Visual Studio 2012, dvakrát klikněte *Movies.mdf* soubor v Průzkumníku řešení otevřete nástroj databáze. Visual Studio Express pro Web se zobrazí databáze Průzkumníka že Visual Studio 2012 se zobrazí v Průzkumníku serveru. Pokud používáte Visual Studio 2010, použijte Průzkumníka objektů systému SQL Server.

V nástroji databáze (Průzkumník databáze, Průzkumníka serveru nebo Průzkumník objektů systému SQL Server), klikněte pravým tlačítkem na `MovieDBContext` a vyberte **odstranit** odpojení filmy databáze.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Přejděte zpět do Průzkumníka řešení. Klikněte pravým tlačítkem na *Movies.mdf* soubor a vyberte **odstranit** k odebrání databáze filmy.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Sestavení aplikace a ujistěte se, že nejsou žádné chyby.

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.

![Přidat Pack Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

V **Konzola správce balíčků** v okno `PM>` řádku zadejte "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Enable-Migrations** příkaz (viz výše) vytvoří *Configuration.cs* soubor v nové *migrace* složky.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Otevře se Visual Studio *Configuration.cs* souboru. Nahraďte `Seed` metoda v *Configuration.cs* soubor s následujícím kódem:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Klikněte pravým tlačítkem na červenou vlnovkou řádku pod `Movie` a vyberte **vyřešit** pak **pomocí** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Díky tomu přidá následující příkaz using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Kód volání první migrace `Seed` metoda po každé migraci (tedy volání **update-database** v konzole Správce balíčků), a tato metoda aktualizace řádků, které již byl vložen a vloží je, pokud jejich Nemáte ještě neexistuje.


**Stisknutím kombinace kláves CTRL-SHIFT-B a tím projekt sestavit.** (Následující postup se nezdaří, pokud vaše není v tomto okamžiku sestavení.)

Dalším krokem je vytvoření `DbMigration` třídu pro počáteční migrace. Tato migrace vytvoří novou databázi, která je důvod, proč můžete odstranit *movie.mdf* soubor v předchozím kroku.

V **Konzola správce balíčků** okno, zadejte příkaz "Přidat migrace počáteční" k vytvoření počáteční migrace. Název "Počáteční" libovolný a slouží k názvu souboru migrace vytvořili.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrace Code First vytvoří další soubor třídy ve *migrace* složky (s názvem *{DateStamp}\_Initial.cs* ), a tato třída obsahuje kód, který vytvoří schématu databáze. Název souboru migrace je předem vyřešili s časovým razítkem usnadní řazení. Zkontrolujte *{DateStamp}\_Initial.cs* souboru, obsahuje pokyny pro vytvoření tabulky filmy pro film databáze. Při aktualizaci databáze v podle pokynů níže, tento *{DateStamp}\_Initial.cs* spustí a vytvoření souboru schéma databáze. Pak se **počáteční hodnoty** metoda spustí k naplnění databáze s testovacích datech.

V **Konzola správce balíčků**, zadejte příkazu "update-database" k vytvoření databáze a spusťte **počáteční hodnoty** metoda.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Pokud dojde k chybě, která označuje tabulka již existuje a nelze vytvořit, je možné, že jste spustili aplikaci po odstranění databáze a před provedení `update-database`. V takovém případě odstraňte *Movies.mdf* znovu a zkuste zopakovat `update-database` příkaz. Pokud stále dojde k chybě, odstraňte složku migrations a obsah, pak spusťte s pokyny uvedenými v horní části této stránky (která je odstranit *Movies.mdf* soubor pak pokračujte Enable-Migrations).

Spusťte aplikaci a přejděte do */Movies* adresy URL. Se zobrazí data počáteční hodnoty.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti hodnocení filmu modelu

Začněte přidáním nové `Rating` vlastnost, která má existující `Movie` třídy. Otevřete *Models\Movie.cs* souboru a přidejte `Rating` vlastnost podobné následujícímu:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Kompletní `Movie` třídy nyní vypadá podobně jako následující kód:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Sestavení aplikace pomocí **sestavení** &gt; **sestavení film** nabídky příkazu nebo stisknutím kombinace kláves CTRL-SHIFT-B.

Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.cshtml* a *\Views\Movies\Create.cshtml* zobrazit šablony, aby bylo možné zobrazit nové `Rating`vlastnost v okně prohlížeče.

Otevřete*\Views\Movies\Index.cshtml* souboru a přidejte `<th>Rating</th>` právě po záhlaví sloupce **cena** sloupce. Pak přidejte `<td>` sloupce u konce šablony k vykreslení `@item.Rating` hodnotu. Níže je co aktualizované *Index.cshtml* zobrazit šablonu vypadá takto:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Dále otevřete *\Views\Movies\Create.cshtml* souboru a přidejte následující značku u konce formuláře. To vykresluje textové pole tak, aby při vytváření nové film můžete zadat hodnocení.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Nyní po aktualizaci kódu aplikace, které podporují nové `Rating` vlastnost.

Nyní spusťte aplikaci a přejděte do */Movies* adresy URL. Když to uděláte, ale se zobrazí jednu z těchto chyb:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Protože se tato chyba zobrazuje aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze. (Není žádná `Rating` sloupec v tabulce databáze.)


Řešení chyby několika způsoby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu. Tento přístup je velmi praktické, když active vývoj pro testovací databázi. umožňuje rychle společně momentální schéma modelu a databáze. Nevýhodou, ale je, že přijdete o stávající data v databázi – tak můžete *nemáte* chcete použít tuto metodu na produkční databázi! Pomocí inicializátoru automaticky počáteční hodnoty databázi s daty test je často produktivní způsob, jak vyvíjet aplikace. Další informace o inicializátory databáze Entity Framework najdete v tématu tní Dykstra [kurz ASP.NET MVC nebo Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu. Výhodou tohoto přístupu je, že zachováte data. Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.
3. Použijte migrace Code First k aktualizaci schématu databáze.


V tomto kurzu použijeme migrace Code First.

Aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnotu pro nový sloupec. Otevřete soubor Migrations\Configuration.cs a přidání hodnocení pole pro každý objekt film.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkaz:

`add-migration AddRatingMig`

`add-migration` Příkaz zjistí migraci framework sloužící ke zkoumání aktuálního modelu film s aktuální schéma film databáze a vytvořit nezbytného kódu migrace databáze do nového modelu. AddRatingMig libovolný a slouží k názvu souboru migrace. Je užitečné pro krok migrace smysluplný název.

Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` odvozené třídy a v `Up` metoda vidíte kód, který vytvoří nový sloupec.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Sestavte řešení a potom zadejte příkaz "update-database" v **Konzola správce balíčků** okno.

Následující obrázek ukazuje výstup v **Konzola správce balíčků** okno (razítka data předřazení AddRatingMig bude jiný.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Znovu spusťte aplikaci a přejděte na adresu URL /Movies. Zobrazí se nové pole hodnocení.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Klikněte **vytvořit nový** odkaz na přidání nové videa. Všimněte si, že můžete přidat hodnocení.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Klikněte na tlačítko **vytvořit**. Nové film, včetně hodnocení, se zobrazí v filmy výpis:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Měli byste také přidat `Rating` pole pro úpravy, podrobnosti a SearchIndex šablon zobrazení.

Můžete zadat příkaz "update-database" v **Konzola správce balíčků** znovu okno a žádné změny by být provedeny, protože schéma neodpovídá modelu.

V této části jste viděli, jak můžete upravit objekty modelu a synchronizujte databázi s změny. Také jste zjistili, způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, můžete zkusit scénáře. V dalším kroku podíváme, jak můžete přidat do třídy modelu bohatší logiku ověření a povolit některé obchodní pravidla, která budou vynucena.

>[!div class="step-by-step"]
[Předchozí](examining-the-edit-methods-and-edit-view.md)
[další](adding-validation-to-the-model.md)
