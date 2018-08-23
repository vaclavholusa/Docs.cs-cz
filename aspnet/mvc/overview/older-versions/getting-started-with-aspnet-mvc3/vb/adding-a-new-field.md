---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Přidání nového pole do modelu filmů a databázové tabulky (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 816660aff696c64948e6ca9daca6632cc9d58082
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754534"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Přidání nového pole do modelu filmů a databázové tabulky (VB)
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
> Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu. [Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/adding-a-new-field.md) tohoto kurzu.


V této části budete provádět některé změny tříd modelu a zjistěte, jak můžete aktualizovat schéma databáze tak, aby odpovídaly změny modelu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Přidání vlastnosti do hodnocení filmů modelu

Začněte přidáním nového `Rating` vlastnost ke stávající `Movie` třídy. Otevřít *Movie.cs* a přidejte `Rating` vlastnost podobný následujícímu:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

Kompletní `Movie` třídy teď vypadá jako v následujícím kódu:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Znovu zkompilovat aplikaci pomocí **ladění** &gt; **sestavení film** příkazu nabídky.

Teď, když jste aktualizovali `Model` třídy, je také potřeba aktualizovat *\Views\Movies\Index.vbhtml* a *\Views\Movies\Create.vbhtml* zobrazení šablon pro podporu nového `Rating`vlastnost.

Otevřít<em>\Views\Movies\Index.vbhtml</em> a přidejte `<th>Rating</th>` hned za záhlaví sloupce <strong>cena</strong> sloupec. Pak přidejte `<td>` sloupec blíží ke konci šablonu k vykreslení `@item.Rating` hodnotu. Níže je co aktualizované <em>Index.vbhtml</em> zobrazit šablonu vypadá jako:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Dále otevřete *\Views\Movies\Create.vbhtml* a přidejte následující kód na konci formuláře. Tím zkopírujete textové pole tak, aby hodnocení můžete zadat, když se vytvoří nová videa.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Správa modelů a rozdíly ve schématu databáze

Teď když jste aktualizovali kód aplikace pro podporu nového `Rating` vlastnost.

Nyní spusťte aplikaci a přejděte */Movies* adresy URL. Když toto provedete, se však zobrazí chybová zpráva:

![](adding-a-new-field/_static/image1.png)

Tato chyba se zobrazuje, protože aktualizovaný `Movie` třídy modelu v aplikaci je nyní liší od schématu `Movie` tabulky existující databáze. (Neexistuje žádný `Rating` sloupec v tabulce databáze.)

Ve výchozím nastavení při použití platformy Entity Framework Code First automaticky vytvořit databázi, jako jste to udělali dříve v tomto kurzu Code First přidá tabulku do databáze pro sledování, zda je synchronizovaný s tříd modelu, které byly vygenerovány z schéma databáze. Pokud nejsou synchronizované, Entity Framework vyvolá chybu. Díky tomu je snadněji sledovat problémy při vývoji, který může jinak pouze pro vás (pomocí skrytého chyby) v době běhu. Tato funkce kontroluje se synchronizace je, co způsobí, že chybová zpráva, který se má zobrazit, které jste viděli.

Existují dva přístupy k vyřešení chyby:

1. Máte rozhraní Entity Framework automaticky vyřadit a znovu vytvořit databázi založené na nové schéma třídy modelu. Tento přístup je příliš pohodlné při aktivním vývoji v testovací databázi, protože umožňuje rychlý rozvoj schématu modelu a databáze společně. Nevýhodou, je však dojít ke ztrátě existujících dat v databázi, tak můžete *není* chcete použít tento postup u provozní databáze!
2. Explicitně upravte schéma stávající databázi tak, aby odpovídalo tříd modelu. Výhodou tohoto přístupu je, že zachováte vaše data. Můžete tuto změnu provést buď ručně, nebo tak, že vytvoříte databázi změnit skript.

V tomto kurzu použijeme první přístup, budete mít Entity Framework Code First automaticky znovu vytvořit databázi kdykoli změny modelu.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Automatické opětovné vytvoření databáze na změny modelu

Umožňuje aktualizovat aplikaci tak, aby Code First automaticky sníží a znovu vytvoří databázi, můžete kdykoli změnit model pro aplikaci.

> [!NOTE] 
> 
> **Upozornění** byste měli povolit tento přístup automaticky vyřadit a znovu vytvořit databázi pouze při použití databázi vývoj nebo testování a *nikdy* u provozní databáze, která obsahuje reálná data. Použití na provozním serveru může způsobit ztrátu dat.


V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a pak vyberte **třídy**.

![](adding-a-new-field/_static/image2.png)

Název třídy &quot;MovieInitializer&quot;. Aktualizace `MovieInitializer` třídy tak, aby obsahovala následující kód:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

`MovieInitializer` Třída určuje, zda by měla být databáze používá model vyřadit a automaticky znovu vytvořena Pokud nikdy změnit tříd modelu. Tento kód obsahuje `Seed` metoda zadat některá data výchozí automaticky přidat až do databáze, když vytvořili (nebo opětovném vytváření). To poskytuje vhodný způsob, jak naplnit databázi s ukázkovými daty, aniž by bylo potřeba ručně přidejte do ní pokaždé, když provedete modelu změnit.

Teď, když jste definovali `MovieInitializer` třídy, je vhodné nastavit tak, aby pokaždé, když je aplikace spuštěná, zkontroluje, jestli se liší od schématu databáze třídy modelu. Pokud ano, můžete spustit inicializátor znovu vytvořit databázi podle modelu a naplnit databázi s ukázkovými daty.

Otevřít *Global.asax* soubor, který je v kořenovém adresáři `MvcMovies` projektu:

*Global.asax* soubor obsahuje třídu, která definuje celé aplikace pro projekt a obsahuje `Application_Start` obslužná rutina události, která se spouští při prvním spuštění aplikace.

Najít `Application_Start` metoda a přidejte volání do `Database.SetInitializer` na začátku metody, jak je znázorněno níže:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

`Database.SetInitializer` Jste právě přidali označuje, že databáze používané `MovieDBContext` instance by měl automaticky odstranit a znovu vytvořen, pokud se schéma a databáze se neshodují. A protože jste viděli, bude také naplnit databázi s ukázkovými daty, která je zadána v `MovieInitializer` třídy.

Zavřít *Global.asax* souboru.

Znovu spusťte aplikaci a přejděte */Movies* adresy URL. Při spuštění aplikace zjistí, zda jejich struktura model už odpovídá schématu databáze. Automaticky znovu vytvoří databázi tak, aby odpovídaly struktuře nový model a naplní databázi s ukázková videa:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Klikněte na tlačítko **vytvořit nový** odkaz na přidání nového videa. Všimněte si, že můžete přidat hodnocení.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Klikněte na tlačítko **vytvořit**. Tento nový film, včetně hodnocení, zobrazí se nově ve výpisu:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

V této části jste viděli, jak můžete upravit objekty modelu a udržovat synchronizované s změny databáze. Také jste se naučili způsob, jak naplnit nově vytvořenou databázi s ukázkovými daty, takže si můžete vyzkoušet scénáře. V dalším kroku Podívejme se na jak můžete přidat bohatší logiku ověřování na třídy modelu a povolit některé obchodní pravidla, která budou vynucena.

> [!div class="step-by-step"]
> [Předchozí](examining-the-edit-methods-and-edit-view.md)
> [další](adding-validation-to-the-model.md)
