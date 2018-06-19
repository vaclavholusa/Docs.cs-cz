---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Přístup k vašemu modelu datům z řadiče | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d3dfa079c334e04f368531456ec2ec4e9728f893
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873554"
---
<a name="accessing-your-models-data-from-a-controller"></a>Přístup k vašemu modelu datům z řadiče
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

V této části vytvoříte novou `MoviesController` třídy a napsat kód, který načte data pro film a zobrazí v prohlížeči pomocí šablony zobrazení.

**Sestavení aplikace** potom přejděte k dalšímu kroku. Pokud nemáte sestavení aplikace, budete dojde k chybě přidáním řadiče.

V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složku a pak klikněte na tlačítko **přidat**, pak **řadič**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

V **přidat vygenerované uživatelské rozhraní** dialogové okno, klikněte na tlačítko **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework**a potom klikněte na **přidat**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Vyberte **Movie (MvcMovie.Models)** pro třídu modelu.
- Vyberte **MovieDBContext (MvcMovie.Models)** pro třída kontextu dat.
- Pro daný název Kontroleru zadejte **MoviesController**.

  Následující obrázek ukazuje dialog dokončené.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Klikněte na tlačítko **přidat**. (Pokud dojde k chybě, budete pravděpodobně nebyla sestavení aplikace před zahájením přidání řadiče.) Visual Studio vytvoří následující soubory a složky:

- *MoviesController.cs* v soubor *řadiče* složky.
- A *Views\Movies* složky.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, a *Index.cshtml* v novém *Views\Movies* složky.

Visual Studio automaticky vytvoří [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení pro vás (Automatická tvorba metody akce CRUD a zobrazení se označuje jako generování uživatelského rozhraní). Nyní máte plně funkční webovou aplikaci, která umožňuje vytvářet, seznam, upravit a odstranit film položky.

Spusťte aplikaci a klikněte na **MVC film** odkaz (nebo vyhledejte `Movies` řadiče připojením */Movies* adresy URL v adresním řádku prohlížeče). Vzhledem k tomu, že se aplikace spoléhá na výchozí směrování (definované v *aplikace\_Start\RouteConfig.cs* soubor), požadavek prohlížeče `http://localhost:xxxxx/Movies` se směruje na výchozí `Index` metody akce `Movies` řadiče. Jinými slovy, požadavek prohlížeče `http://localhost:xxxxx/Movies` je efektivně stejný jako požadavek prohlížeče `http://localhost:xxxxx/Movies/Index`. Výsledkem je prázdný seznam filmy, protože zatím jste nepřidali žádné.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Vytváření film

Vyberte **vytvořit nový** odkaz. Zadejte některé podrobnosti o film a poté klikněte **vytvořit** tlačítko.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Nelze zadat desetinných míst nebo čárkami pole cena. pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárkou (&quot;,&quot;) pro desetinné čárky a formát data neanglických USA, musíte zahrnout *globalize.js* a konkrétní  *cultures/Globalize.cultures.js* souboru (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) a JavaScript používat `Globalize.parseFloat`. I budete ukazují, jak to udělat v dalším kurzu. Teď zadejte jenom celá čísla, jako je 10.


Kliknutím **vytvořit** tlačítko způsobí, že formulář odeslat na server, kde film informace se ukládají v databázi. Potom budete přesměrováni na */Movies* adresu URL, kde se můžete podívat na nově vytvořený video ve výpisu.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Vytvořte několik další film položky. Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.

## <a name="examining-the-generated-code"></a>Zkoumání generovaný kód

Otevřete *Controllers\MoviesController.cs* soubor a zkontrolujte vygenerovaného `Index` metoda. Část řadičem film s `Index` metoda jsou uvedeny níže.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Požadavek na `Movies` vrátí všechny položky v `Movies` tabulce a pak předá výsledky do `Index` zobrazení. Následující řádek z `MoviesController` třída vytvoří kontext databáze film, jak je popsáno výše. Dotaz, upravovat a odstraňovat filmy, můžete použít film kontext databáze.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silného typu modely a @model – klíčové slovo

V tomto kurzu jste viděli, jak řadič může předat data nebo objekty zobrazení šablony pomocí `ViewBag` objektu. `ViewBag` Je dynamický objekt, který představuje pohodlný způsob pozdní vazbou předávat informace k zobrazení.

MVC rovněž poskytuje možnost předat *důrazně* zadali objekty, které chcete zobrazit šablonu. Tento přístup silného typu umožňuje lepší kompilaci vracení se změnami kódu a bohatší [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) v editoru Visual Studio. Mechanismus generování uživatelského rozhraní v sadě Visual Studio používá tento přístup (to znamená, předávání *důrazně* typu modelu) s `MoviesController` třídy a zobrazení šablony při jeho vytvoření metody a zobrazení.

V *Controllers\MoviesController.cs* zkontrolujte vygenerovaného souboru `Details` metoda. `Details` Metoda jsou uvedeny níže.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id` Parametru se obecně předá jako data trasy, například `http://localhost:1234/movies/details/1` řadičem nastaví řadič film, akce, která `details` a `id` na 1. Můžete také předat v id s řetězcem dotazu následujícím způsobem:

`http://localhost:1234/movies/details?id=1`

Pokud `Movie` nenajde, instanci `Movie` model předán `Details` zobrazení:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Zkontrolujte obsah *Views\Movies\Details.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Zahrnutím `@model` příkaz v horní části souboru šablony zobrazení, můžete zadat typ objektu, která očekává zobrazení. Pokud jste vytvořili řadičem film, Visual Studio automaticky zahrnuty následující `@model` příkaz v horní části *Details.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

To `@model` – direktiva umožňuje přístup k video, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu. Například v *Details.cshtml* šablony, kód předá pro každé pole film `DisplayNameFor` a [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) pomocné objekty HTML s silného typu `Model` objektu. `Create` a `Edit` metody a zobrazit šablony předat také film objektu modelu.

Zkontrolujte *Index.cshtml* zobrazit šablonu a `Index` metoda v *MoviesController.cs* souboru. Všimněte si, jak kód vytvoří [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektu při volání `View` Pomocná metoda v `Index` metody akce. Tento kód pak předá `Movies` ze seznamu `Index` metody akce k zobrazení:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Pokud jste vytvořili řadičem film, Visual Studio automaticky zahrnuty následující `@model` příkaz v horní části *Index.cshtml* souboru:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

To `@model` – direktiva umožňuje přístup k seznamu filmy, které kontroleru předaná do zobrazení pomocí pomocí `Model` objekt, který je silného typu. Například v *Index.cshtml* šablony, kód prochází filmy nástrojem `foreach` příkaz přes silného typu `Model` objektu:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Protože `Model` je silného typu objektu (jako `IEnumerable<Movie>` objektu), každý `item` objekt ve smyčce je zadán jako `Movie`. Mezi další výhody to znamená, že získat kontrola kompilace kódu a úplné podporu technologie IntelliSense v editoru kódu:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Práce s LocalDB serveru SQL

Entity Framework Code First zjistil, že připojovací řetězec databáze, která byla poskytnuta ukazuje `Movies` databáze, který nebyl ještě neexistuje, Code First vytvoří databázi automaticky. Můžete ověřit, že je vytvořeno podle *aplikace\_Data* složky. Pokud nevidíte *Movies.mdf* souboru, klikněte na tlačítko **zobrazit všechny soubory** tlačítka na **Průzkumníku řešení** nástrojů, klikněte na tlačítko **aktualizovat** tlačítko a potom rozbalte *aplikace\_Data* složky.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Klikněte dvakrát na *Movies.mdf* otevřete **PRŮZKUMNÍKA serveru**, pak rozbalte **tabulky** složku pro najdete v tabulce filmy. Poznámka: na ikonu klíče vedle ID. Ve výchozím nastavení budou EF vlastnost s názvem ID primární klíč. Další informace o EF a MVC naleznete v části kurzu vynikající tní Dykstra na [MVC a EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **zobrazit Data tabulky** chcete zobrazit data, které jste vytvořili.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Klikněte pravým tlačítkem myši `Movies` tabulky a vyberte **otevřete definici tabulky** zobrazíte v tabulce struktury této Entity Framework Code First automaticky vytvořeny.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Všimněte si jak schéma `Movies` tabulka mapuje `Movie` třídy, které jste vytvořili dříve. Entity Framework Code First automaticky vytvoří tento schématu na základě vaší `Movie` třídy.

Až budete hotovi, zavřete připojení kliknutím pravým tlačítkem *MovieDBContext* a výběrem **zavřít připojení**. (Pokud nemáte ukončení připojení, může dojde k chybě při příštím spuštění projektu).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data. V dalším kurzu jsme budete zkontrolujte zbytek automaticky generovaný kód a přidejte `SearchIndex` metoda a `SearchIndex` zobrazení, které umožňuje hledat filmy v této databázi. Další informace o používání rozhraní Entity Framework s MVC najdete v tématu [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Předchozí](creating-a-connection-string.md)
> [další](examining-the-edit-methods-and-edit-view.md)
