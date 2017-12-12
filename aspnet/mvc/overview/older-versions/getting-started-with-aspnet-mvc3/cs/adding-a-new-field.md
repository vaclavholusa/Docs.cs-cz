---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: "Přidání nové pole do modelu film a tabulky (C#) | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: c10f3be30a92a605c34fa1c56fa3691389374beb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Přidání nové pole do modelu film a tabulky (C#)
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


V této části můžete provést některé změny třídy modelu a zjistěte, jak můžete aktualizovat schéma databáze tak, aby odpovídaly změny modelu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti hodnocení filmu modelu

Začněte přidáním nové `Rating` vlastnost, která má existující `Movie` třídy. Otevřete *Movie.cs* souboru a přidejte `Rating` vlastnost podobné následujícímu:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Kompletní `Movie` třídy nyní vypadá podobně jako následující kód:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Znovu zkompiluje aplikace pomocí **ladění** &gt; **sestavení film** příkazu nabídky.

Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.cshtml* a *\Views\Movies\Create.cshtml* zobrazí šablony za účelem podpory nové `Rating`vlastnost.

Otevřete *\Views\Movies\Index.cshtml* souboru a přidejte `<th>Rating</th>` právě po záhlaví sloupce **cena** sloupce. Pak přidejte `<td>` sloupce u konce šablony k vykreslení `@item.Rating` hodnotu. Níže je co aktualizované *Index.cshtml* zobrazit šablonu vypadá takto:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Dále otevřete *\Views\Movies\Create.cshtml* souboru a přidejte následující značku u konce formuláře. To vykresluje textové pole tak, aby při vytváření nové film můžete zadat hodnocení.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Správa modelu a rozdíly schématu databáze

Nyní po aktualizaci kódu aplikace, které podporují nové `Rating` vlastnost.

Nyní spusťte aplikaci a přejděte do */Movies* adresy URL. Když to uděláte, se ale zobrazí následující chyba:

![](adding-a-new-field/_static/image1.png)

Protože se tato chyba zobrazuje aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze. (Není žádná `Rating` sloupec v tabulce databáze.)

Ve výchozím nastavení při použití Entity Framework Code First automaticky vytvořit databázi, stejně jako dříve v tomto kurzu Code First přidá tabulku k databázi chcete-li sledovat, jestli je synchronizována s třídy modelu, který se vygeneroval ze schématu databáze. Pokud nejsou synchronizované, rozhraní Entity Framework vrátí chybu. To usnadňuje sledovat problémy v čase vývoj, který může jinak pouze pro vás (podle skrytého chyby) v době běhu. Funkci Kontrola synchronizace je co způsobí, že se chybová zpráva, který se má zobrazit, jste viděli.

Řešení chyby dvěma způsoby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi na základě nové třídy schématu modelu. Tento přístup je velmi vhodné při provádění active vývoj v testovací databázi, protože umožňuje rychle společně momentální schéma modelu a databáze. Nevýhodou, ale je, že přijdete o stávající data v databázi – tak můžete *nemáte* chcete použít tuto metodu na produkční databázi!
2. Explicitně změnit schéma z existující databáze tak, aby odpovídala třídy modelu. Výhodou tohoto přístupu je, že zachováte data. Můžete tuto změnu provést buď ručně, nebo vytvořením databáze změnit skriptu.

V tomto kurzu použijeme prvním přístupem – budete mít Entity Framework Code First automaticky znovu vytvořit databázi kdykoliv změny modelu.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Automaticky znovu vytvořit databázi na změny modelu

Umožňuje aktualizovat aplikaci tak, aby Code First automaticky zahodí a znovu vytvoří databázi kdykoliv změnit modelu pro aplikace.

> [!NOTE] 
> 
> **Upozornění** byste měli povolit tento přístup automaticky vyřadit a znovu vytvořit databázi pouze v případě, že používáte vývoj nebo testování databáze, a *nikdy* na provozní databázi, která obsahuje skutečná data. Pomocí na provozním serveru může způsobit ztrátu dat.


V **Průzkumníku řešení**, klikněte pravým tlačítkem *modely* složky, vyberte **přidat**a potom vyberte **třída**.

![](adding-a-new-field/_static/image2.png)

Název třídy "MovieInitializer". Aktualizace `MovieInitializer` třída obsahuje následující kód:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

`MovieInitializer` Třída určuje, že by měl být databáze používá model vyřadit a automaticky znovu vytvoří Pokud někdy změnit třídy modelu. Tento kód obsahuje `Seed` metoda zadat některá data výchozí automaticky přidat do databáze žádný čas ji vytvořit (nebo znovu vytvořit). To poskytuje vhodný způsob, jak naplnění databáze ukázková data, aniž by bylo potřeba ručně přidejte do ní pokaždé, když provedete model změnit.

Teď, když jste definovali `MovieInitializer` třídy, budete se muset propojit se tak, aby pokaždé, když je aplikace spuštěná, se ověří, zda třídy modelu jsou odlišné od schématu v databázi. Pokud ano, můžete spustit inicializátoru znovu vytvořit databázi na model a pak naplnit databázi s ukázkovými daty.

Otevřete *Global.asax* soubor, který je v kořenovém adresáři `MvcMovies` projektu:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

*Global.asax* soubor obsahuje třídu, která definuje bude celá aplikace pro projekt a obsahuje `Application_Start` obslužné rutiny události, která se spouští při prvním spuštění aplikace.

Přidejme dva příkazy using do horní části souboru. První odkazuje na obor názvů Entity Framework, a druhá odkazuje na obor názvů kde naše `MovieInitializer` třídy život:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Vyhledejte `Application_Start` metoda a přidejte volání `Database.SetInitializer` na začátku metodu, jak je uvedeno níže:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

`Database.SetInitializer` Příkaz jste právě přidali označuje, že databáze použít pomocí `MovieDBContext` instance by měl být automaticky odstraní a znovu vytvoří Pokud schéma a databáze se neshodují. A protože jste viděli, bude také naplnit databázi s ukázkovými daty, která je zadána v `MovieInitializer` třídy.

Zavřít *Global.asax* souboru.

Znovu spusťte aplikaci a přejděte do */Movies* adresy URL. Při spuštění aplikace zjistí, zda jejich struktura model už odpovídá schématu databáze. Automaticky znovu vytvoří databázi tak, aby odpovídala nové modelovou strukturu a naplní databázi s filmy ukázka:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Klikněte **vytvořit nový** odkaz na přidání nové videa. Všimněte si, že můžete přidat hodnocení.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Klikněte na tlačítko **vytvořit**. Nové film, včetně hodnocení, se zobrazí v filmy výpis:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

V této části jste viděli, jak můžete upravit objekty modelu a synchronizujte databázi s změny. Také jste zjistili, způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, můžete zkusit scénáře. V dalším kroku podíváme, jak můžete přidat do třídy modelu bohatší logiku ověření a povolit některé obchodní pravidla, která budou vynucena.

>[!div class="step-by-step"]
[Předchozí](examining-the-edit-methods-and-edit-view.md)
[další](adding-validation-to-the-model.md)
