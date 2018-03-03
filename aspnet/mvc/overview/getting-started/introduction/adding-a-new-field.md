---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: "Přidání nové pole | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 453fbf68aa2f3a1d9ea708355c06c53d4f1eabd0
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
<a name="adding-a-new-field"></a>Přidání nové pole
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

V této části používáte migrace Code First Entity Framework pro migraci některé změny do třídy modelu tak, že použití této změny do databáze.

Ve výchozím nastavení při použití Entity Framework Code First automaticky vytvořit databázi, stejně jako dříve v tomto kurzu Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze. Pokud nejsou synchronizované, rozhraní Entity Framework vrátí chybu. To usnadňuje sledovat problémy v čase vývoj, který může jinak pouze pro vás (podle skrytého chyby) v době běhu.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Nastavení migrace Code First pro změny modelu

Přejděte do Průzkumníka řešení. Klikněte pravým tlačítkem na *Movies.mdf* soubor a vyberte **odstranit** k odebrání databáze filmy. Pokud nevidíte *Movies.mdf* souboru, klikněte na **zobrazit všechny soubory** vidíte níže v osnově červenou ikonu.

![](adding-a-new-field/_static/image1.png)

Sestavení aplikace a ujistěte se, že nejsou žádné chyby.

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet** a potom **Konzola správce balíčků**.

![Přidat Pack Man](adding-a-new-field/_static/image2.png)

V **Konzola správce balíčků** v okno `PM>` řádku zadejte

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-Migrations** příkaz (viz výše) vytvoří *Configuration.cs* soubor v nové *migrace* složky.

![](adding-a-new-field/_static/image4.png)

Otevře se Visual Studio *Configuration.cs* souboru. Nahraďte `Seed` metoda v *Configuration.cs* soubor s následujícím kódem:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Pozastavte ukazatel myši nad red vlnovkou řádku pod `Movie` a klikněte na tlačítko `Show Potential Fixes` a pak klikněte na **pomocí** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Díky tomu přidá následující příkaz using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE] 
> 
> Kód volání první migrace `Seed` metoda po každé migraci (tedy volání **update-database** v konzole Správce balíčků), a tato metoda aktualizace řádků, které již byl vložen a vloží je, pokud jejich Nemáte ještě neexistuje.
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda v následujícím kódu provede operaci "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Protože [počáteční hodnoty](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metoda se spouští se každých migrace, data, nelze právě vložit, protože řádky, které se pokoušíte přidat bude po první migrace, která vytváří databázi již existovat. "[Upsert](http://en.wikipedia.org/wiki/Upsert)" operaci brání chyb, které by mohlo dojít, pokud se pokusíte vložit řádek, který již existuje, ale přepíše všechny změny dat, která může provedení při testování aplikace. Pro testovací data v některé tabulky nemusí Chcete to tak proběhlo: v některých případech při změně dat při testování vaše změny zůstat po aktualizace databáze. V takovém případě budete chtít provést operace podmíněného insert: Vložit řádek pouze v případě, že ještě neexistuje.   
>   
> První parametr předaný [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metoda určuje vlastnost, která má použít pro kontrolu, pokud řádek již existuje. Pro testovací data film, který zadáte `Title` vlastnost lze použít pro tento účel, protože v nadpisu každé v seznamu je jedinečný:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Tento kód předpokládá, že se jedinečné názvy. Pokud chcete ručně přidat duplicitní název, získáte následující výjimka při příštím provedení migrace.   
>   
>  *Sekvence obsahuje více než jeden element.*  
>   
> Další informace o [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodu, najdete v části [postará s EF 4.3 AddOrUpdate metoda](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Stisknutím kombinace kláves CTRL-SHIFT-B a tím projekt sestavit.** (Následující kroky se nezdaří, pokud není v tomto okamžiku sestavení.)

Dalším krokem je vytvoření `DbMigration` třídu pro počáteční migrace. Tato migrace vytvoří novou databázi, která je důvod, proč můžete odstranit *movie.mdf* soubor v předchozím kroku.

V **Konzola správce balíčků** okno, zadejte příkaz `add-migration Initial` vytvořit počáteční migrace. Název "Počáteční" libovolný a slouží k názvu souboru migrace vytvořili.

![](adding-a-new-field/_static/image6.png)

Migrace Code First vytvoří další soubor třídy ve *migrace* složky (s názvem *{DateStamp}\_Initial.cs* ), a tato třída obsahuje kód, který vytvoří schématu databáze. Název souboru migrace je předem vyřešili s časovým razítkem usnadní řazení. Zkontrolujte *{DateStamp}\_Initial.cs* souboru, obsahuje pokyny pro vytvoření `Movies` tabulku pro film databáze. Při aktualizaci databáze v podle pokynů níže, tento *{DateStamp}\_Initial.cs* soubor bude spustit a vytvořit schéma databáze. Pak se **počáteční hodnoty** metoda spustí k naplnění databáze s testovacích datech.

V **Konzola správce balíčků**, zadejte příkaz `update-database` vytvořit databázi a spustit `Seed` metoda.

![](adding-a-new-field/_static/image7.png)

Pokud dojde k chybě, která označuje tabulka již existuje a nelze vytvořit, je možné, že jste spustili aplikaci po odstranění databáze a před provedení `update-database`. V takovém případě odstraňte *Movies.mdf* znovu a zkuste zopakovat `update-database` příkaz. Pokud stále dojde k chybě, odstraňte složku migrations a obsah, pak spusťte s pokyny uvedenými v horní části této stránky (která je odstranit *Movies.mdf* soubor pak pokračujte Enable-Migrations). Pokud se pořád zobrazí chyba, otevřete Průzkumník objektů systému SQL Server a odebrat databázi ze seznamu.

Spusťte aplikaci a přejděte do */Movies* adresy URL. Se zobrazí data počáteční hodnoty.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti hodnocení filmu modelu

Začněte přidáním nové `Rating` vlastnost, která má existující `Movie` třídy. Otevřete *Models\Movie.cs* souboru a přidejte `Rating` vlastnost podobné následujícímu:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Kompletní `Movie` třídy nyní vypadá podobně jako následující kód:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Sestavení aplikace (Ctrl + Shift + B).

Protože jste přidali nové pole do `Movie` třídy, je také potřeba aktualizovat vazby *seznamu povolených* , tato vlastnost budou zahrnuty. Aktualizace `bind` atribut pro `Create` a `Edit` akce metody pro zahrnutí `Rating` vlastnost:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Je také potřeba aktualizovat zobrazit šablony, aby bylo možné zobrazit, vytvořit a upravit nové `Rating` vlastnost v okně prohlížeče.

Otevřete *\Views\Movies\Index.cshtml* souboru a přidejte `<th>Rating</th>` právě po záhlaví sloupce **cena** sloupce. Pak přidejte `<td>` sloupce u konce šablony k vykreslení `@item.Rating` hodnotu. Níže je co aktualizované *Index.cshtml* zobrazit šablonu vypadá takto:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Dále otevřete *\Views\Movies\Create.cshtml* souboru a přidejte `Rating` pole s následující kód highlighed. To vykresluje textové pole tak, aby při vytváření nové film můžete zadat hodnocení.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Nyní po aktualizaci kódu aplikace, které podporují nové `Rating` vlastnost.

Spusťte aplikaci a přejděte do */Movies* adresy URL. Když to uděláte, ale se zobrazí jednu z těchto chyb:

![](adding-a-new-field/_static/image9.png)  
  
Model zálohování kontext 'MovieDBContext' se od vytvoření databáze změnil. Zvažte použití migrace Code First k aktualizaci databáze (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Protože se tato chyba zobrazuje aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze. (Není žádná `Rating` sloupec v tabulce databáze.)


Řešení chyby několika způsoby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu. Tento přístup je velmi praktické již v rané fázi v cyklu vývoje, pokud byste active vývoj pro testovací databázi. umožňuje rychle společně momentální schéma modelu a databáze. Nevýhodou, ale je, že přijdete o stávající data v databázi – tak můžete *nemáte* chcete použít tuto metodu na produkční databázi! Pomocí inicializátoru automaticky počáteční hodnoty databázi s daty test je často produktivní způsob, jak vyvíjet aplikace. Další informace o inicializátory databáze Entity Framework najdete v tématu [kurz ASP.NET MVC nebo Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu. Výhodou tohoto přístupu je, že zachováte data. Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.
3. Použijte migrace Code First k aktualizaci schématu databáze.


V tomto kurzu použijeme migrace Code First.

Aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnotu pro nový sloupec. Otevřete soubor Migrations\Configuration.cs a přidání hodnocení pole pro každý objekt film.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkaz:

`add-migration Rating`

`add-migration` Příkaz zjistí migraci framework sloužící ke zkoumání aktuálního modelu film s aktuální schéma film databáze a vytvořit nezbytného kódu migrace databáze do nového modelu. Název *hodnocení* libovolný a slouží k názvu souboru migrace. Je užitečné pro krok migrace smysluplný název.

Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMigration` odvozené třídy a v `Up` metoda vidíte kód, který vytvoří nový sloupec.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Sestavte řešení a potom zadejte `update-database` v příkazu **Konzola správce balíčků** okno.

Následující obrázek ukazuje výstup v **Konzola správce balíčků** okno (datum razítko předřazení *hodnocení* se budou lišit.)

![](adding-a-new-field/_static/image11.png)

Znovu spusťte aplikaci a přejděte na adresu URL /Movies. Zobrazí se nové pole hodnocení.

![](adding-a-new-field/_static/image12.png)

Klikněte **vytvořit nový** odkaz na přidání nové videa. Všimněte si, že můžete přidat hodnocení.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Klikněte na tlačítko **vytvořit**. Nové film, včetně hodnocení, se zobrazí v filmy výpis:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Teď, když na projekt používá migrace, nebudete muset odpojení databáze, když přidáte nové pole nebo jinak aktualizovat schéma. V další části jsme budete provést další změny schématu a aktualizaci databáze pomocí migrace.

Měli byste také přidat `Rating` pole pro úpravy, podrobnosti a odstraňování šablon zobrazení.

Můžete zadat příkaz "update-database" v **Konzola správce balíčků** znovu okno a žádný kód migrace by spustit, protože schéma neodpovídá modelu. Ale systémem "update-database" se spustí `Seed` metoda znovu, a pokud jste změnili počáteční hodnoty dat, změny budou ztraceny, protože `Seed` metoda upserts data. Další informace o `Seed` metoda v tní Dykstra oblíbených [kurz ASP.NET MVC nebo Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

V této části jste viděli, jak můžete upravit objekty modelu a synchronizujte databázi s změny. Také jste zjistili, způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, můžete zkusit scénáře. To se právě rychlý úvod do Code First najdete v tématu [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Podrobnější kurz týkající se předmět. V dalším kroku podíváme, jak můžete přidat do třídy modelu bohatší logiku ověření a povolit některé obchodní pravidla, která budou vynucena.

>[!div class="step-by-step"]
[Předchozí](adding-search.md)
[další](adding-validation.md)
