---
uid: mvc/overview/getting-started/introduction/adding-search
title: "Hledání | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 116f681e14af0a09a4eb1502ef9f057c5db2f97d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="search"></a>Hledat
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Přidání metodu Search a zobrazení vyhledávání

V této části přidáte možnosti vyhledávání `Index` metody akce, která umožňuje vyhledávání filmy genre nebo název.

## <a name="updating-the-index-form"></a>Aktualizace indexu formuláře

Začněte tím, že aktualizace `Index` metoda akce ke stávající `MoviesController` třídy. Zde je kód:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

První řádek `Index` metoda vytvoří následující [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotazu a vyberte filmy:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

Dotaz je definována v tomto okamžiku, ale dosud nebyla spuštěna v databázi.

Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na základě hodnoty řetězec pro hledání, pomocí následujícího kódu:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

`s => s.Title` Je výše uvedený kód [výrazu Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Lambdas se používají v na základě metod [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotazuje jako argumenty pro standardní dotaz operátor metody, jako [kde](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metoda použitá ve výše uvedeném kódu. Dotazy LINQ nebudou provedeny, když jsou definovány nebo když jsou upraveny voláním metody `Where` nebo `OrderBy`. Místo toho při provádění dotazu je odložení, což znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je ve skutečnosti vstupní přes nebo [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda je volána. V `Search` ukázce dotaz se spouštějí v tom *Index.cshtml* zobrazení. Další informace o provádění odložené dotazů najdete v tématu [provádění dotazu](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> [Obsahuje](https://msdn.microsoft.com/library/bb155125.aspx) metoda běží v databázi, není c# výše uvedený kód. V databázi [obsahuje](https://msdn.microsoft.com/library/bb155125.aspx) mapuje [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), což je malá a velká písmena.

Teď můžete aktualizovat `Index` zobrazení, které se uživateli zobrazí formulář.

Spusťte aplikaci a přejděte do *nebo filmy nebo Index*. Připojit řetězec dotazu, jako `?searchString=ghost` na adresu URL. Filtrované filmy jsou zobrazeny.

![SearchQryStr](adding-search/_static/image1.png)

Pokud změníte podpis `Index` metoda tak, aby měl parametr s názvem `id`, `id` parametr bude odpovídat `{id}` zástupný symbol pro výchozí směruje sada v *aplikace\_Start\ Soubor RouteConfig.cs* souboru.

[!code-json[Main](adding-search/samples/sample4.json)]

Původní `Index` metoda vypadá takto::

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Upravenou `Index` metoda vypadat takto:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Název hledání můžete nyní předat jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.

![](adding-search/_static/image2.png)

Nelze však budou uživatelé chcete upravit adresu URL pokaždé, když chtějí hledat film. Ano, teď můžete přidáte uživatelského rozhraní, můžete je filtrovat filmy. Pokud jste změnili podpis `Index` metoda k testování jak předat parametr ID vázané na trasy ho změnit tak, aby vaše `Index` metoda přijímá řetězcový parametr s názvem `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Otevřete *Views\Movies\Index.cshtml* souborů a právě po `@Html.ActionLink("Create New", "Create")`, přidejte značku formuláře zvýrazněná níže:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm` Pomocné rutiny vytvoří počáteční `<form>` značky. `Html.BeginForm` Pomocná způsobí, že formulář post sám na sebe, když uživatel odešle formulář kliknutím **filtru** tlačítko.

Visual Studio 2013 má dobrý zlepšování při zobrazení a úpravy souborů zobrazení. Při spuštění aplikace s soubor zobrazení otevřít, Visual Studio 2013 vyvolá metoda akce kontroleru správné zobrazíte zobrazení.

![](adding-search/_static/image3.png)

Pomocí zobrazení indexu otevřete v sadě Visual Studio (jak je znázorněno na obrázku výše), klepněte na pev.cenu F5 nebo F5 spusťte aplikaci a pak zkuste vyhledat film.

![](adding-search/_static/image4.png)

Neexistuje žádné `HttpPost` přetížení z `Index` metoda. Není nutné, protože metoda není změny stavu aplikace, právě filtrování dat.

Můžete přidat následující `HttpPost Index` metoda. V takovém případě by odpovídat původce volání akce `HttpPost Index` metody a `HttpPost Index` metoda by spustit jak je znázorněno na obrázku níže.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Ale i v případě, přidejte tuto `HttpPost` verzi `Index` metoda, existuje omezení v tom, jak to všechny byl implementován. Představte si, že chcete bookmark konkrétní hledání nebo chcete poslat odkaz přátel, chcete-li zobrazit stejný filtrovaný seznam filmy mohou klepnutím. Všimněte si, že adresa URL požadavku HTTP POST je stejný jako adresu URL pro požadavek GET (localhost:xxxxx nebo filmy nebo Index) – chybí informace o vyhledávání v adrese URL sám sebe. Práva nyní informace řetězec hledání je odeslána na server jako hodnotu pole formuláře. To znamená, že nelze zaznamenáte tyto informace vyhledávání bookmark nebo poslat přátel v adrese URL.

Řešení, je použít přetížení `BeginForm` který určuje, že požadavek POST měli přidat informace o vyhledávání na adresu URL, a že by měl být směrované na `HttpGet` verzi `Index` metoda. Nahradit existující bez parametrů `BeginForm` metoda s následující kód:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Teď při odesílání vyhledávání, adresa URL obsahuje řetězec dotazu vyhledávání. Hledání budou také moct `HttpGet Index` metody akce, i když máte `HttpPost Index` metoda.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Přidání vyhledávání podle Genre

Pokud jste přidali `HttpPost` verzi `Index` metoda, odstraňte jej.

V dalším kroku přidáte funkci tak, aby uživatelé vyhledejte filmy podle genre. Nahraďte `Index` metoda následujícím kódem:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Tato verze `Index` metoda přebírá další parametr, a to `movieGenre`. Vytvoření první několika řádků kódu `List` objekt pro uložení žánry film z databáze.

Následující kód je dotaz LINQ, který načte všechny žánry z databáze.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

Kód používá `AddRange` metoda obecná `List` kolekce odlišné žánry přidat do seznamu. (Bez `Distinct` modifikátor, by byl přidán duplicitní žánry – například komedie by byl přidán dvakrát v naše ukázka). Kód pak uloží seznam žánry v `ViewBag.MovieGenre` objektu. Ukládání dat kategorie (takové film genre společnosti) jako [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) objekt v `ViewBag`, pak se typické postup pro aplikace MVC, přístup k datům kategorii v rozevírací pole se seznamem.

Následující kód ukazuje, jak zkontrolovat `movieGenre` parametr. Pokud není prázdná, omezí kód další dotaz filmy omezit vybrané filmy k zadané genre.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Jak jsme uvedli dříve, dotaz se nespouští na databáze, dokud je vstupní seznamu video přes (který se stane v zobrazení po `Index` metoda akce vrací).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Přidání značek k zobrazení Index pro podporu vyhledávání podle Genre

Přidat `Html.DropDownList` pomocníka, který má *Views\Movies\Index.cshtml* souboru, těsně před `TextBox` pomocné rutiny. Dokončený kód je zobrazena níže:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

V následujícím kódu:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

Poskytuje klíč pro parametr "MovieGenre" `DropDownList` pomocná rutina pro vyhledání `IEnumerable<SelectListItem>` v `ViewBag`. `ViewBag` Byly zadané v metodě akce:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

Parametr "Vše" poskytuje popisku. Pokud tuto volbu si prohlédnout v prohlížeči, uvidíte, že jeho atribut "value" je prázdný. Vzhledem k tomu, že naše řadiče pouze filtry `if` řetězec není `null` nebo je prázdná, odesílání prázdnou hodnotu pro `movieGenre` ukazuje všechny žánry.

Můžete také nastavit možnost být vybrán ve výchozím nastavení. Pokud byste chtěli "Komedie" jako výchozí možnost, chcete změnit kód v Kontroleru takto:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Spusťte aplikaci a přejděte do *nebo filmy nebo Index*. Zkuste hledání genre, název film a obě kritéria.

![](adding-search/_static/image8.png)

V této části můžete vytvořit metody akce vyhledávání a zobrazení, která umožní uživatelům vyhledávat podle názvu film a genre. V další části se budete podíváte na tom, jak přidat vlastnost, která má `Movie` modelu a postup přidání inicializátoru, který se automaticky vytvoří testovací databáze.

>[!div class="step-by-step"]
[Předchozí](examining-the-edit-methods-and-edit-view.md)
[další](adding-a-new-field.md)
