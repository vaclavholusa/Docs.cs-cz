---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Zkoumání upravit metody a zobrazení | Microsoft Docs
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 304d3c1efbce8949fd9385529f2a16b07e05ffdf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Zkoumání upravit metody a zobrazení
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části budete zkontrolujte metody generované akce a zobrazení pro film kontroleru. Potom přidáte vlastní vyhledávací stránku.

Spusťte aplikaci a přejděte do `Movies` řadiče připojením */Movies* na adresu URL na panelu Adresa prohlížeče. Podržte ukazatel myši nad **upravit** na adresu URL, která obsahuje odkazy na odkaz.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Upravit** odkaz vygenerovalo `Html.ActionLink` metoda v *Views\Movies\Index.cshtml* zobrazení:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Objekt je pomocné rutiny, který je zveřejněný pomocí vlastnosti na [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) základní třídy. `ActionLink` Metoda pomocníka, který usnadňuje dynamicky generovat hypertextové odkazy HTML, které odkazují na metody akce na řadiče. První argument `ActionLink` metoda je text odkazu k vykreslení (například `<a>Edit Me</a>`). Druhý argument je název metody akce, která bude vyvolána. Konečný argument je [anonymní objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) který generuje data trasy (v tomto případě ID 4).

Vygenerovaný odkaz viz předchozí obrázek je `http://localhost:xxxxx/Movies/Edit/4`. Výchozí trasa (nastavené v *aplikace\_Start\RouteConfig.cs*) trvá vzor adresy URL `{controller}/{action}/{id}`. Proto ASP.NET překládá `http://localhost:xxxxx/Movies/Edit/4` do požadavek na `Edit` metody akce `Movies` řadiče s parametrem `ID` rovna 4. Zkontrolujte následující kód *aplikace\_Start\RouteConfig.cs* souboru.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Také můžete předat parametry metody akce pomocí řetězce dotazu. Například adresa URL `http://localhost:xxxxx/Movies/Edit?ID=4` také předá parametr `ID` 4 k `Edit` metody akce `Movies` řadiče.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Otevřete `Movies` řadiče. Dva `Edit` metody akce, které jsou uvedeny níže.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Všimněte si, druhý `Edit` předchází metody akce `HttpPost` atribut. Tento atribut určuje, že přetížení `Edit` metoda může být volána pouze pro požadavky POST. Je možné aplikovat `HttpGet` atribut prvního upravit metodu, ale který není nutné protože je výchozí hodnota. (Budeme označovat metody akce, které jsou implicitně přiřazené `HttpGet` atribut jako `HttpGet` metody.)

`HttpGet` `Edit` Metoda přebírá parametr ID film, vyhledává film používající rozhraní Entity Framework `Find` metoda a vrátí vybraného videa na zobrazení pro úpravy. Určuje parametr ID [výchozí hodnota](https://msdn.microsoft.com/library/dd264739.aspx) z nula v případě `Edit` metoda je volána bez parametrů. Pokud nelze nalézt film, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) je vrácen. Při generování uživatelského rozhraní systému vytvoření zobrazení pro úpravy, zkontrolován `Movie` třídy a vytvořený kód k vykreslení `<label>` a `<input>` prvky pro každou vlastnost třídy. Následující příklad ukazuje zobrazení upravit, která byla vygenerována:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Všimněte si, jak se má zobrazit šablonu `@model MvcMovie.Models.Movie` příkaz v horní části souboru – tato hodnota určuje, že zobrazení očekává, že model pro zobrazení šablony být typu `Movie`.

Automaticky generovaný kód používá několik *pomocné metody* zefektivnění kód HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocné rutiny zobrazuje název pole (&quot;název&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, nebo &quot;cena &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Vykreslí pomocné rutiny HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocník zobrazí všechny zprávy ověření přidružené k této vlastnosti.

Spusťte aplikaci a přejděte do */Movies* adresy URL. Klikněte na tlačítko **upravit** odkaz. V prohlížeči zobrazte zdroj pro stránku. Kód HTML pro form element jsou uvedeny níže.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>` Prvky jsou v kódu HTML `<form>` element jejichž `action` k vystavování příspěvků na nastavený atribut *nebo filmy nebo upravit* adresy URL. Data formuláře budou odeslány na server při **upravit** po kliknutí na tlačítko.

## <a name="processing-the-post-request"></a>Zpracování požadavku POST

Následující seznam uvádí `HttpPost` verzi `Edit` metody akce.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[Vazač modelu ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) přijímá hodnoty odeslaného formuláře a vytvoří `Movie` objekt, který se předá jako `movie` parametr. `ModelState.IsValid` Metoda ověří, že odeslaná data do formuláře lze použít k úpravě (upravit nebo aktualizace) `Movie` objektu. Pokud jsou data platná, film data jsou uložena do `Movies` kolekce `db(MovieDBContext` instance). Nová data film je uložit do databáze při volání `SaveChanges` metodu `MovieDBContext`. Po uložení dat, kód přesměruje uživatele `Index` metody akce `MoviesController` třídy, které zobrazuje film kolekce, včetně pouze změny.

Pokud odeslaných hodnot nejsou platné, se zobrazí znovu ve formuláři. `Html.ValidationMessageFor` Pomocné rutiny v *Edit.cshtml* zobrazení šablony postará o zobrazení příslušné chybové zprávy.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárkou (&quot;,&quot;) desetinné čárky, je třeba zahrnout *globalize.js* a konkrétní *cultures/globalize.cultures.js* souboru (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) a JavaScript používat `Globalize.parseFloat`. Následující kód ukazuje změny souboru Views\Movies\Edit.cshtml pro práci s &quot;fr-FR&quot; jazyková verze:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Pole decimal může vyžadovat čárkou, není desetinné čárky. Jako dočasné opravu můžete přidat element globalizace projekty kořenovém souboru web.config. Následující kód ukazuje element globalizace jazyková verze nastavena angličtina (Spojené státy).

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Všechny `HttpGet` podobný Princip podle metody. Získají objekt film (nebo seznam objektů, v případě `Index`) a předat zobrazení modelu. `Create` Metoda předá objekt prázdný film zobrazení pro vytváření. Takže v udělat všechny metody, které vytvořit, upravit, odstranit nebo jinak měnit data `HttpPost` přetížení metody. Úprava dat v metodu GET protokolu HTTP je bezpečnostním rizikem, jak je popsáno v příspěvku blogu [ASP.NET MVC Tip #46 – nepoužívejte odkazy odstranit, protože uživatel vytvořit celistvosti](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Úprava dat v metodu GET také porušuje HTTP osvědčené postupy a architektury [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) vzor, který určuje, že požadavky GET nesmí změnit stav vaší aplikace. Jinými slovy provádění operace GET by měl být bezpečné operace, která nemá žádné vedlejší účinky a nezmění trvalá data.

## <a name="adding-a-search-method-and-search-view"></a>Přidání metodu Search a zobrazení vyhledávání

V této části přidáte `SearchIndex` metody akce, která umožňuje vyhledávání filmy genre nebo název. To bude k dispozici pomocí */filmy/SearchIndex* adresy URL. Žádost se zobrazí formuláře HTML, která obsahuje elementy vstupu, které může uživatel zadat, aby bylo možné vyhledat film. Když uživatel formulář odešle, metoda akce se získat hodnoty vyhledávání vystavil uživatele a použití hodnot pro vyhledávání databázi.

## <a name="displaying-the-searchindex-form"></a>Zobrazení SearchIndex formuláře

Začněte přidáním `SearchIndex` metoda akce ke stávající `MoviesController` třídy. Metoda vrátí zobrazení, která obsahuje formuláře HTML. Zde je kód:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

První řádek `SearchIndex` metoda vytvoří následující [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotazu a vyberte filmy:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

Dotaz je definována v tomto okamžiku, ale dosud nebyla spuštěna před úložišti.

Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na základě hodnoty řetězec pro hledání, pomocí následujícího kódu:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

`s => s.Title` Je výše uvedený kód [výrazu Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Lambdas se používají v na základě metod [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotazuje jako argumenty pro standardní dotaz operátor metody, jako [kde](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metoda použitá ve výše uvedeném kódu. Dotazy LINQ nebudou provedeny, když jsou definovány nebo když jsou upraveny voláním metody `Where` nebo `OrderBy`. Místo toho při provádění dotazu je odložení, což znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je ve skutečnosti vstupní přes nebo [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda je volána. V `SearchIndex` ukázku, je dotaz proveden v SearchIndex zobrazení. Další informace o provádění odložené dotazů najdete v tématu [provádění dotazu](https://msdn.microsoft.com/library/bb738633.aspx).

Teď můžete implementovat `SearchIndex` zobrazení, které se uživateli zobrazí formulář. Klepněte pravým tlačítkem myši `SearchIndex` metoda a pak klikněte na tlačítko **přidat zobrazení**. V **přidat zobrazení** dialogovém okně zadejte, že budete předávat `Movie` objekt, který chcete zobrazit šablonu jako jeho třídu modelu. V **vygenerované uživatelské rozhraní šablony** vyberte **seznamu**, pak klikněte na tlačítko **přidat**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Když kliknete **přidat** tlačítko *Views\Movies\SearchIndex.cshtml* vytvoření šablony zobrazení. Vzhledem k tomu, že jste vybrali **seznamu** v **vygenerované uživatelské rozhraní šablony** seznamu, Visual Studio automaticky generovány (vygeneroval) některé výchozí značek v zobrazení. Generování uživatelského rozhraní vytvořit formuláře HTML. Je zkontrolován `Movie` třídy a vytvořený kód k vykreslení `<label>` prvky pro každou vlastnost třídy. Seznam dole ukazuje vytvořit zobrazení, která byla vygenerována:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Spusťte aplikaci a přejděte do */filmy/SearchIndex*. Připojit řetězec dotazu, jako `?searchString=ghost` na adresu URL. Filtrované filmy jsou zobrazeny.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Pokud změníte podpis `SearchIndex` metoda tak, aby měl parametr s názvem `id`, `id` parametr bude shodovat s `{id}` zástupný symbol pro výchozí směruje sada v *Global.asax* souboru.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Původní `SearchIndex` metoda vypadá takto::

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Upravenou `SearchIndex` metoda vypadat takto:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Název hledání můžete nyní předat jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Nelze však budou uživatelé chcete upravit adresu URL pokaždé, když chtějí hledat film. Ano, teď můžete přidáte uživatelského rozhraní, můžete je filtrovat filmy. Pokud jste změnili podpis `SearchIndex` metoda k testování jak předat parametr ID vázané na trasy ho změnit tak, aby vaše `SearchIndex` metoda přijímá řetězcový parametr s názvem `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Otevřete *Views\Movies\SearchIndex.cshtml* souborů a právě po `@Html.ActionLink("Create New", "Create")`, přidejte následující:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

Následující příklad ukazuje část *Views\Movies\SearchIndex.cshtml* souboru se značkami přidané filtrování.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm` Pomocné rutiny vytvoří počáteční `<form>` značky. `Html.BeginForm` Pomocná způsobí, že formulář post sám na sebe, když uživatel odešle formulář kliknutím **filtru** tlačítko.

Spusťte aplikaci a zkuste vyhledat film.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Neexistuje žádné `HttpPost` přetížení z `SearchIndex` metoda. Není nutné, protože metoda není změny stavu aplikace, právě filtrování dat.

Můžete přidat následující `HttpPost SearchIndex` metoda. V takovém případě by odpovídat původce volání akce `HttpPost SearchIndex` metody a `HttpPost SearchIndex` metoda by spustit jak je znázorněno na obrázku níže.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Ale i v případě, přidejte tuto `HttpPost` verzi `SearchIndex` metoda, existuje omezení v tom, jak to všechny byl implementován. Představte si, že chcete bookmark konkrétní hledání nebo chcete poslat odkaz přátel, chcete-li zobrazit stejný filtrovaný seznam filmy mohou klepnutím. Všimněte si, že adresa URL požadavku HTTP POST je stejný jako adresu URL pro požadavek GET (localhost:xxxxx/filmy/SearchIndex) – chybí informace o vyhledávání v adrese URL sám sebe. Práva nyní informace řetězec hledání je odeslána na server jako hodnotu pole formuláře. To znamená, že nelze zaznamenáte tyto informace vyhledávání bookmark nebo poslat přátel v adrese URL.

Řešení, je použít přetížení `BeginForm` který určuje, že požadavek POST měli přidat informace o vyhledávání na adresu URL, a že by měl být směrované na třídy MetadataExchangeClientMode verzi `SearchIndex` metoda. Nahradit existující bez parametrů `BeginForm` metoda následujícím kódem:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Teď při odesílání vyhledávání, adresa URL obsahuje řetězec dotazu vyhledávání. Hledání budou také moct `HttpGet SearchIndex` metody akce, i když máte `HttpPost SearchIndex` metoda.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Přidání vyhledávání podle Genre

Pokud jste přidali `HttpPost` verzi `SearchIndex` metoda, odstraňte jej.

V dalším kroku přidáte funkci tak, aby uživatelé vyhledejte filmy podle genre. Nahraďte `SearchIndex` metoda následujícím kódem:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Tato verze `SearchIndex` metoda přebírá další parametr, a to `movieGenre`. Vytvoření první několika řádků kódu `List` objekt pro uložení žánry film z databáze.

Následující kód je dotaz LINQ, který načte všechny žánry z databáze.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Kód používá `AddRange` metoda obecná `List` kolekce odlišné žánry přidat do seznamu. (Bez `Distinct` modifikátor, by byl přidán duplicitní žánry – například komedie by byl přidán dvakrát v naše ukázka). Kód pak uloží seznam žánry v `ViewBag` objektu.

Následující kód ukazuje, jak zkontrolovat `movieGenre` parametr. Pokud není prázdná, omezí kód další dotaz filmy omezit vybrané filmy k zadané genre.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Přidání značek k zobrazení SearchIndex pro podporu vyhledávání podle Genre

Přidat `Html.DropDownList` pomocníka, který má *Views\Movies\SearchIndex.cshtml* souboru, těsně před `TextBox` pomocné rutiny. Dokončený kód je zobrazena níže:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Spusťte aplikaci a přejděte do */filmy/SearchIndex*. Zkuste hledání genre, název film a obě kritéria.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

V této části posouzení metody akce CRUD a zobrazení generované rozhraní. Můžete vytvořit metody akce vyhledávání a zobrazení, která umožní uživatelům vyhledávat podle názvu film a genre. V další části se budete podíváte na tom, jak přidat vlastnost, která má `Movie` modelu a postup přidání inicializátoru, který se automaticky vytvoří testovací databáze.

> [!div class="step-by-step"]
> [Předchozí](accessing-your-models-data-from-a-controller.md)
> [další](adding-a-new-field-to-the-movie-model-and-table.md)
