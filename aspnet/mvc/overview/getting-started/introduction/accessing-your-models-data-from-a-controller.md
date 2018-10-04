---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Přístup k datům modelu z Kontroleru | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 00731fbc0d578afa2df881b205170fb6a4f686e1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576271"
---
<a name="accessing-your-models-data-from-a-controller"></a>Přístup k datům modelu z Kontroleru
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data o filmech a zobrazí v prohlížeči pomocí zobrazení šablony.

**Sestavení aplikace** před přechodem k dalšímu kroku. Pokud není sestavení aplikace, získáte k chybě při přidávání kontroleru.

V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složku a potom klikněte na **přidat**, pak **řadič**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

V **přidat vygenerované uživatelské rozhraní** dialogové okno, klikněte na tlačítko **kontroler MVC 5 se zobrazeními, používá nástroj Entity Framework**a potom klikněte na tlačítko **přidat**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Vyberte **Movie (MvcMovie.Models)** pro třídu modelu.
- Vyberte **MovieDBContext (MvcMovie.Models)** pro třídy datového kontextu.
- Zadejte název řadiče **MoviesController**.

  Následující obrázek ukazuje dokončený dialogového okna.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Klikněte na tlačítko **přidat**. (Pokud dojde k chybě, pravděpodobně nebyla sestavení aplikace před zahájením přidat kontroler.) Visual Studio vytvoří následující soubory a složky:

- *MoviesController.cs* soubor *řadiče* složky.
- A *Views\Movies* složky.
- *Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, a *Index.cshtml* na novém *Views\Movies* složky.

Visual Studio automaticky vytvoří [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvoření, čtení, aktualizace a odstranění) metody akce a zobrazení pro vás (automatické vytváření metody akcí CRUD a zobrazení se označuje jako generování uživatelského rozhraní). Teď máte plně funkční webovou aplikaci, která umožňuje vytvořit, seznam, upravovat a odstraňovat položky video.

Spusťte aplikaci a klikněte na **MVC Movie** odkaz (nebo vyhledejte `Movies` řadič přidáním */Movies* na adresu URL do adresního řádku prohlížeče). Protože aplikace se spoléhá na výchozí směrování (definované v *aplikace\_Start\RouteConfig.cs* souboru), žádost prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí hodnotu `Index` metody akce `Movies` kontroleru. Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je v podstatě totéž jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`. Výsledkem je prázdný seznam filmy, protože ještě jste nepřidali žádné.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Vytváření video

Vyberte **vytvořit nový** odkaz. Zadejte podrobnosti o videa a poté klikněte **vytvořit** tlačítko.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Nebudete moci zadejte desetinné čárky nebo čárkami v poli pro cenu. pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárku (&quot;,&quot;) pro desetinné čárky a USA retweetovat neanglické formáty kalendářního data, musíte zahrnout *globalize.js* a konkrétní  *cultures/Globalize.cultures.js* souboru (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) a JavaScript, který chcete použít `Globalize.parseFloat`. Ukážeme si jak to provést v dalším kurzu. Teď zadejte celá čísla, jako je 10.


Kliknutím **vytvořit** tlačítko způsobí, že formulář, který se má publikovat na server, kde je video informace uloženy v databázi. Pak budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video v seznamu.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Vytvořte několik další položky video. Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.

## <a name="examining-the-generated-code"></a>Zkoumání generovaného kódu

Otevřít *Controllers\MoviesController.cs* souboru a prozkoumání generované `Index` metody. Část kontroler filmů s `Index` metoda je uveden níže.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Požadavek na `Movies` kontroler vrací všechny položky v `Movies` tabulky a potom předává výsledky `Index` zobrazení. Následující řádek ze `MoviesController` třídy vytvoří kontext databáze filmů, jak je popsáno výše. Kontext databáze filmů slouží k dotazování, upravovat a odstraňovat videa.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modely se silnými typy a @model – klíčové slovo

Dříve v tomto kurzu jste viděli, jak kontroleru můžete předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu. `ViewBag` Je dynamický objekt, který představuje pohodlný způsob s pozdní vazbou k předávání informací do zobrazení.

MVC nabízí také možnost předat *důrazně* objektů zobrazit šablonu typem. Tento přístup silného typu umožňuje lepší kompilace kontroly kódu a bohatší [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) v editoru sady Visual Studio. Tento přístup používá mechanismus generování uživatelského rozhraní v sadě Visual Studio (to znamená, předávání *důrazně* zadaný model) s `MoviesController` šablony třídy a zobrazení při vytvoření rovnou metody a zobrazení.

V *Controllers\MoviesController.cs* zkontrolujte vygenerovaný soubor `Details` metody. `Details` Metoda je uveden níže.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id` Parametr je obvykle předán jako data trasy, například `http://localhost:1234/movies/details/1` film kontroler, akci, která se nastaví kontroler `details` a `id` na hodnotu 1. Můžete také předat ID s řetězcem dotazu následujícím způsobem:

`http://localhost:1234/movies/details?id=1`

Pokud `Movie` nachází instance `Movie` modelu je předán `Details` zobrazení:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Zkontrolovat obsah *Views\Movies\Details.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Zahrnutím `@model` příkazu v horní části souboru šablony zobrazení, můžete určit typ objektu, který očekává, že zobrazení. Při vytvoření kontroleru video Visual Studio automaticky zahrnuty následující `@model` příkazu v horní části *Details.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

To `@model` – direktiva umožňuje přístup k video, které kontroleru předána do zobrazení podle používání `Model` objekt, který je silně typováno. Například v *Details.cshtml* šablony, že kód předá jednotlivých polí filmů a `DisplayNameFor` a [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocných rutin HTML se silnými typy `Model` objektu. `Create` a `Edit` metody a zobrazení šablony také předat objekt modelu videa.

Zkontrolujte *Index.cshtml* zobrazit šablonu a `Index` metodu *MoviesController.cs* souboru. Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektu při volání `View` pomocnou metodu v `Index` metody akce. Tento kód pak předá `Movies` ze seznamu `Index` metody akce k zobrazení:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Při vytvoření kontroleru video Visual Studio automaticky zahrnuty následující `@model` příkazu v horní části *Index.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

To `@model` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předána do zobrazení podle používání `Model` objekt, který je silně typováno. Například v *Index.cshtml* šablony, kód prochází videa prováděním `foreach` příkaz přes silného typu `Model` objektu:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Protože `Model` objektu je silně typováno (jako `IEnumerable<Movie>` objektu), každý `item` objektu ve smyčce je zadán jako `Movie`. Kromě dalších výhod to znamená získání kompilace Kontrola kódu a plnou podporu technologie IntelliSense v editoru kódu:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Práce s verzí SQL Server LocalDB

Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla k dispozici na který je odkazováno `Movies` databázi, která tenkrát neexistovaly, takže Code First vytvořit databáze automaticky. Můžete ověřit, že se vytvořil zobrazením *aplikace\_Data* složky. Pokud se nezobrazí *Movies.mdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítko **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a pak rozbalte *aplikace\_Data* složky.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Dvakrát klikněte na panel *Movies.mdf* otevřete **PRŮZKUMNÍKA serveru**, potom rozbalte **tabulky** složky najdete v tabulce filmy. Všimněte si, na ikonu klíče vedle ID. Ve výchozím nastavení EF způsobí, že vlastnost s názvem ID primárního klíče. Další informace o EF a MVC najdete v kurzu vynikající Dykstra Petr na [MVC a EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Klikněte pravým tlačítkem myši `Movies` tabulce a vybrat **zobrazit Data tabulky** k zobrazení dat, které jste vytvořili.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Klikněte pravým tlačítkem myši `Movies` tabulce a vybrat **Otevřít definici tabulky** zobrazíte tabulky struktury této Entity Framework Code First pro vás vytvořili.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Všimněte si, že jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve. Entity Framework Code First automaticky vytvoří toto schéma na základě vašich `Movie` třídy.

Jakmile budete hotovi, ukončete připojení kliknutím pravým tlačítkem *MovieDBContext* a vyberete **zavřít připojení**. (Pokud není ukončení připojení, pravděpodobně dojde k chybě při příštím spuštění projektu).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Teď máte databázi a stránkami zobrazit, upravit, aktualizovat a odstraňovat data. V dalším kurzu vytvoříme zkontrolujte zbytek automaticky generovaný kód a přidat `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat videa v této databázi. Další informace o použití rozhraní Entity Framework s MVC najdete v tématu [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Předchozí](creating-a-connection-string.md)
> [další](examining-the-edit-methods-and-edit-view.md)
