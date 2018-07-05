---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Přidání nového pole do modelu a tabulky Movie | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 25a29e783f02e66e1713d8120eb526e1a02961a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366473"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Přidání nového pole do modelu a tabulky Movie
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části použijete migrace Entity Framework Code First k migraci některých změn do tříd modelu, tak použití této změny do databáze.

Ve výchozím nastavení při použití platformy Entity Framework Code First automaticky vytvořit databázi, jako jste to udělali dříve v tomto kurzu Code First přidá tabulku do databáze pro sledování, zda je synchronizovaný s tříd modelu, které byly vygenerovány z schéma databáze. Pokud nejsou synchronizované, Entity Framework vyvolá chybu. Díky tomu je snadněji sledovat problémy při vývoji, který může jinak pouze pro vás (pomocí skrytého chyby) v době běhu.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Nastavení migrace Code First pro změny modelu

Pokud používáte sadu Visual Studio 2012, dvakrát klikněte *Movies.mdf* souboru z Průzkumníku řešení otevřete nástroj pro databáze. Visual Studio Express for Web se zobrazí Průzkumník databáze, že Visual Studio 2012 se zobrazí Průzkumník serveru. Pokud používáte Visual Studio 2010, použijte Průzkumník objektů systému SQL Server.

V databázi nástroje (Průzkumník databáze, Průzkumníku serveru nebo Průzkumník objektů systému SQL Server), klikněte pravým tlačítkem na `MovieDBContext` a vyberte **odstranit** pro odpojení databáze filmů.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Přejděte zpět do Průzkumníku řešení. Klikněte pravým tlačítkem na *Movies.mdf* a vyberte možnost **odstranit** odebrání databáze filmů.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Vytvoření aplikace, abyste měli jistotu, že nejsou žádné chyby.

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven** a potom **Konzola správce balíčků**.

![Přidat balíček Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

V **Konzola správce balíčků** v okna `PM>` řádku zadejte "Enable-migrace - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

**Povolení migrace** vytvoří příkaz (popsaný výš) *Configuration.cs* souboru v novém *migrace* složky.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio otevře *Configuration.cs* souboru. Nahradit `Seed` metodu *Configuration.cs* souboru následujícím kódem:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Klikněte pravým tlačítkem na řádku červená vlnovka pod `Movie` a vyberte **vyřešit** pak **pomocí** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Tím se přidají následující příkaz using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Kód volá první migrace `Seed` za každou migraci (tedy volání **aktualizace databáze** v konzole Správce balíčků), a tato metoda aktualizuje řádky, které již byl vložen a vloží je, pokud jsou dosud neexistují.


**Stisknutím klávesy CTRL-SHIFT-B a sestavte projekt.** (Následující kroky se nezdaří, pokud vaše není v tomto okamžiku sestavení.)

Dalším krokem je vytvoření `DbMigration` třídu pro počáteční migraci. Tato migrace na vytvoří novou databázi, proto můžete odstranit *movie.mdf* souboru v předchozím kroku.

V **Konzola správce balíčků** okno, zadejte příkaz "Přidat migrace počáteční" k vytvoření počáteční migraci. Název "Počáteční" libovolný a slouží k pojmenování souboru migrace vytvořili.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrace Code First vytvoří další soubor třídy v *migrace* složky (s názvem *{DateStamp}\_Initial.cs* ), a tato třída obsahuje kód, který vytvoří schéma databáze. Název souboru migraci je pevně předem s časovým razítkem Nápověda k řazení. Zkontrolujte *{DateStamp}\_Initial.cs* souboru, obsahuje pokyny k vytvoření tabulky filmy pro databáze filmů. Při aktualizaci databáze v pokynech níže to *{DateStamp}\_Initial.cs* souboru spustí a vytvořit schéma databáze. Pak bude **počáteční hodnoty** metoda spustí k naplnění databáze se testovací data.

V **Konzola správce balíčků**, zadejte příkaz "update databáze" k vytvoření databáze a spusťte **počáteční hodnoty** metody.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Pokud dojde k chybě, která určuje již existuje a nedá se vytvořit tabulku, je možné, že jste spustili aplikaci po odstranění databáze a předtím, než jste spustili `update-database`. V takovém případě odstranit *Movies.mdf* soubor znovu a zkuste to znovu `update-database` příkazu. Pokud stále dojde k chybě, odstraňte složku migrace a obsah, pak spusťte v pokynech v horní části této stránky (to je odstranit *Movies.mdf* souborů pak přejděte k povolení migrace).

Spusťte aplikaci a přejděte */Movies* adresy URL. Počáteční data se zobrazí.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti do hodnocení filmů modelu

Začněte přidáním nového `Rating` vlastnost ke stávající `Movie` třídy. Otevřít *Models\Movie.cs* a přidejte `Rating` vlastnost podobný následujícímu:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Kompletní `Movie` třídy teď vypadá jako v následujícím kódu:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Sestavení aplikace pomocí **sestavení** &gt; **sestavení film** nabídky příkazu, nebo stisknutím klávesy CTRL-SHIFT-B.

Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.cshtml* a *\Views\Movies\Create.cshtml* zobrazení šablon zobrazíte nové `Rating`vlastností v okně prohlížeče.

Otevřít<em>\Views\Movies\Index.cshtml</em> a přidejte `<th>Rating</th>` hned za záhlaví sloupce <strong>cena</strong> sloupec. Pak přidejte `<td>` sloupec blíží ke konci šablonu k vykreslení `@item.Rating` hodnotu. Níže je co aktualizované <em>Index.cshtml</em> zobrazit šablonu vypadá jako:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Dále otevřete *\Views\Movies\Create.cshtml* a přidejte následující kód na konci formuláře. Tím zkopírujete textové pole tak, aby hodnocení můžete zadat, když se vytvoří nová videa.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Teď když jste aktualizovali kód aplikace pro podporu nového `Rating` vlastnost.

Nyní spusťte aplikaci a přejděte */Movies* adresy URL. Když toto provedete, ale zobrazí se vám některé z následujících chyb:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Tato chyba se zobrazuje, protože aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze. (Neexistuje žádný `Rating` sloupec v tabulce databáze.)


Řešení chyby několika způsoby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi založené na nové schéma třídy modelu. Tento přístup je příliš pohodlné při aktivním vývoji pro testovací databázi. umožňuje rychlý rozvoj schématu modelu a databáze společně. Nevýhodou, je však dojít ke ztrátě existujících dat v databázi, tak můžete *není* chcete použít tento postup u provozní databáze! Použití inicializátoru automaticky naplnit databázi daty testu je často produktivní způsob, jak vyvíjet aplikace. Další informace o databázi inicializátory Entity Framework naleznete v tématu Petr Dykstra [kurz ASP.NET MVC a Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu. Výhodou tohoto přístupu je, že zachováte vaše data. Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.
3. Pomocí migrace Code First aktualizovat schéma databáze.


Pro účely tohoto kurzu používáme migrace Code First.

Aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnoty pro nový sloupec. Otevřete soubor Migrations\Configuration.cs a přidat hodnocení pole do každé video.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkaz:

`add-migration AddRatingMig`

`add-migration` Příkaz říká rozhraní framework migrace sloužící ke zkoumání aktuální model filmů s aktuální schéma databáze filmů a vytvářet kód potřebné k migraci databáze na nový model. AddRatingMig je volitelný a slouží k pojmenování souboru migrace. Je vhodné použít smysluplný název kroku migrace.

Po dokončení tohoto příkazu, Visual Studio otevře soubor třídy, která definuje nový `DbMIgration` odvozené třídy a `Up` metoda se zobrazí kód, který vytvoří nový sloupec.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Sestavte řešení a potom zadejte příkaz "aktualizace databáze" v **Konzola správce balíčků** okna.

Následující obrázek znázorňuje výstup v **Konzola správce balíčků** okno (časové razítko předřazení AddRatingMig bude lišit.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Znovu spusťte aplikaci a přejděte na adresu URL /Movies. Uvidíte nové pole hodnocení.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Klikněte na tlačítko **vytvořit nový** odkaz na přidání nového videa. Všimněte si, že můžete přidat hodnocení.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Klikněte na tlačítko **vytvořit**. Tento nový film, včetně hodnocení, zobrazí se nově ve výpisu:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Měli byste také přidat `Rating` pole pro úpravy, podrobnosti a SearchIndex zobrazit šablony.

Můžete například zadat příkaz "aktualizace databáze" v **Konzola správce balíčků** znovu okno a žádné změny byste nastavit, protože schéma odpovídá schématu modelu.

V této části jste viděli, jak můžete upravit objekty modelu a udržovat synchronizované s změny databáze. Také jste se naučili způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, takže si můžete vyzkoušet scénáře. V dalším kroku Podívejme se na jak můžete přidat bohatší logiku ověřování na třídy modelu a povolit některé obchodní pravidla, která budou vynucena.

> [!div class="step-by-step"]
> [Předchozí](examining-the-edit-methods-and-edit-view.md)
> [další](adding-validation-to-the-model.md)
