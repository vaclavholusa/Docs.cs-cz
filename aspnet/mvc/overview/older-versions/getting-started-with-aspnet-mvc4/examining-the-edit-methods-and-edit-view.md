---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Zkoumání metod Edit a zobrazení pro úpravy | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: f8d66343ad74e45f167701f405c5e6e2fef0dd13
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824371"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Zkoumání metod Edit a zobrazení pro úpravy
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části budete prozkoumat metody generované akce a zobrazení kontroleru video. Potom přidáte vlastní stránku vyhledávání.

Spusťte aplikaci a přejděte `Movies` řadič přidáním */Movies* na adresu URL do adresního řádku prohlížeče. Podržte ukazatel myši nad **upravit** na adresu URL, která odkazuje na odkaz.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Upravit** byl vytvořen odkaz `Html.ActionLink` metodu *Views\Movies\Index.cshtml* zobrazení:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Pomocné rutiny, která je vystavena pomocí vlastnosti ve je objekt [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) základní třídy. `ActionLink` Metody pomocné rutiny usnadňuje dynamicky generovat hypertextové odkazy HTML, které jsou propojeny do metody akce v řadičích. První argument `ActionLink` metoda je text odkazu pro vykreslení (například `<a>Edit Me</a>`). Druhým argumentem je název metody akce, která se má vyvolat. Konečný argument je [anonymní objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , který generuje data trasy (v tomto případě ID 4).

Vygenerovaný odkaz je vidět na předchozím obrázku je `http://localhost:xxxxx/Movies/Edit/4`. Výchozí trasa (v *aplikace\_Start\RouteConfig.cs*) trvá vzor adresy URL `{controller}/{action}/{id}`. Proto převede ASP.NET `http://localhost:xxxxx/Movies/Edit/4` do požadavku na `Edit` metody akce `Movies` řadiče s parametrem `ID` rovna 4. Zkontrolujte následující kód z *aplikace\_Start\RouteConfig.cs* souboru.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Můžete také předat parametry metody akce pomocí řetězce dotazu. Například adresa URL `http://localhost:xxxxx/Movies/Edit?ID=4` také předá parametr `ID` 4 `Edit` metody akce `Movies` kontroleru.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Otevřít `Movies` kontroleru. Dva `Edit` níže jsou uvedené metody akce.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Všimněte si, že druhá `Edit` předchází metody akce `HttpPost` atribut. Tento atribut určuje, že přetížení `Edit` metodu lze vyvolat pouze pro požadavky POST. Můžete použít `HttpGet` atribut na první upravit metodu, ale to není nezbytné vzhledem k tomu, že je výchozí hodnota. (Budeme odkazovat na metody akce, které jsou přiřazeny implicitně `HttpGet` atribut jako `HttpGet` metody.)

`HttpGet` `Edit` Metoda přijímá parametr ID film, vyhledá videa pomocí Entity Frameworku `Find` metoda a vrátí vybraného videa do zobrazení pro úpravy. Určuje parametr ID [výchozí hodnota](https://msdn.microsoft.com/library/dd264739.aspx) z nula, pokud `Edit` metoda je volána bez parametrů. Pokud nelze najít videa, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) je vrácena. Když systém generování uživatelského rozhraní zobrazení pro úpravy, prozkoumat `Movie` třídy a vytvořený kód pro vykreslení `<label>` a `<input>` prvky pro každou vlastnost třídy. Následující příklad ukazuje zobrazení pro úpravy, která byla vygenerována:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Všimněte si, jak se má zobrazit šablonu `@model MvcMovie.Models.Movie` příkazu v horní části souboru, toto nastavení určuje, že zobrazení očekává, že model pro zobrazení šablony, která má být typu `Movie`.

Automaticky generovaný kód používá několik *pomocné metody* zjednodušit značka jazyka HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Název pole, zobrazí pomocná (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;žánr&quot;, nebo &quot;cena &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Vykreslí pomocné rutiny HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocné rutiny zobrazí všechny zprávy ověření přidružené k této vlastnosti.

Spusťte aplikaci a přejděte */Movies* adresy URL. Klikněte na tlačítko **upravit** odkaz. V prohlížeči zobrazte zdroj stránky. Kód HTML pro element formuláře je uveden níže.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>` Prvky jsou v kódu HTML `<form>` elementu jehož `action` atribut je nastaven na Odeslat požadavek POST */filmy/úpravy* adresy URL. Publikuje se data formuláře na serveru při **upravit** po kliknutí na tlačítko.

## <a name="processing-the-post-request"></a>Zpracování požadavku POST

Následující seznam ukazuje `HttpPost` verzi `Edit` metody akce.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[Vazač modelu ASP.NET MVC](https://msdn.microsoft.com/magazine/hh781022.aspx) vezme hodnoty odeslaného formuláře a vytvoří `Movie` objektu, který je předán jako `movie` parametru. `ModelState.IsValid` Metoda ověří, že odeslaná data do formuláře je možné upravit (úpravy nebo aktualizace) `Movie` objektu. Pokud jsou data platná, data o filmech uložená `Movies` kolekce `db(MovieDBContext` instance). Nová data o filmech uložená v databázi voláním `SaveChanges` metoda `MovieDBContext`. Po uložení dat, kód přesměruje uživatele `Index` metody akce `MoviesController` třídy, které ukazuje kolekce filmů, včetně pouze změny.

Pokud odeslaných hodnot nejsou platné, se zobrazí znovu ve formuláři. `Html.ValidationMessageFor` Pomocné rutiny v *Edit.cshtml* zobrazení šablony postará o zobrazení odpovídající chybové zprávy.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárku (&quot;,&quot;) desetinné čárky, je třeba zahrnout *globalize.js* a konkrétní *cultures/globalize.cultures.js* souboru (z [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) a JavaScript, který chcete použít `Globalize.parseFloat`. Následující kód ukazuje změny souboru Views\Movies\Edit.cshtml pracovat &quot;fr-FR&quot; jazykové verze:


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Pole desítkových může vyžadovat čárku, není desetinné čárky. Jako dočasné opravu přidáte element globalizace projekty kořenovém souboru web.config. Následující kód ukazuje elementu globalization jazyková verze nastavena na angličtinu Spojených států.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Všechny `HttpGet` metody podobné tvar. Dostanou video (nebo seznam objektů, v případě `Index`) a předána do zobrazení modelu. `Create` Metoda předává objekt prázdný film zobrazení pro vytváření. Všechny metody, které vytvořit, upravit, odstranit nebo jinak upravit data, proveďte `HttpPost` přetížení metody. Úpravy dat v metodě GET protokolu HTTP je bezpečnostním rizikem, jak je popsáno v příspěvku blogu [ASP.NET MVC Tip #46 – nepoužívejte odstranit odkazy, protože uživatel vytvořit bezpečnostní díry](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Úpravy dat v metodě GET HTTP osvědčené postupy vaší architektury a také porušuje [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) vzor, který určuje, že požadavky GET by neměly měnit stav vaší aplikace. Jinými slovy provádění operace GET by měl být bezpečný provoz, který nemá žádné vedlejší účinky a nemění trvalá data.

## <a name="adding-a-search-method-and-search-view"></a>Přidání vyhledávací metody a zobrazení vyhledávání

V této části přidáte `SearchIndex` metody akce, která umožňuje hledat filmy podle žánru nebo názvu. To bude k dispozici pomocí */filmy/SearchIndex* adresy URL. Žádost se zobrazí formulář HTML, který obsahuje vstupní prvky, které může uživatel zadat, aby bylo možné hledat videa. Jakmile uživatel formulář odešle, metoda akce získat hodnoty vyhledávání publikovaných uživatelem, který se hodnoty slouží k vyhledání databáze.

## <a name="displaying-the-searchindex-form"></a>Zobrazení formuláře SearchIndex

Začněte přidáním `SearchIndex` metody akce ke stávající `MoviesController` třídy. Metoda vrátí zobrazení obsahující formulář HTML. Zde je kód:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

První řádek `SearchIndex` metoda vytvoří následující [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotaz pro výběr videa:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

Dotaz je definován v tomto okamžiku, ale nebyl dosud spuštěn proti úložišti.

Pokud `searchString` parametr obsahuje řetězec, dotaz filmy je upravit tak, aby filtrování na základě hodnoty hledaný řetězec, pomocí následujícího kódu:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

`s => s.Title` Je výše uvedený kód [výraz Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Výrazy lambda se používají v založených na volání metody [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotazuje jako argumenty pro standardní metody operátoru dotazu, jako [kde](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) metodu používanou ve výše uvedeném kódu. Dotazy LINQ nejsou provedeny, když jsou definovány, nebo při jejich změně voláním metody `Where` nebo `OrderBy`. Místo toho provádění dotazu je odloženo, což znamená, že vyhodnocení výrazu je odloženo jeho očekávané hodnoty ve skutečnosti procházen nebo [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda je volána. V `SearchIndex` ukázkové spuštění dotazu v zobrazení SearchIndex. Další informace o odložený dotaz, naleznete v tématu [provádění dotazu](https://msdn.microsoft.com/library/bb738633.aspx).

Teď můžete implementovat `SearchIndex` zobrazení, které se uživateli zobrazí formulář. Klepněte pravým tlačítkem myši `SearchIndex` metody a pak klikněte na tlačítko **přidat zobrazení**. V **přidat zobrazení** dialogového okna zadejte, že budete předávat `Movie` objektu, který chcete zobrazit šablonu jako jeho třída modelu. V **šablona Scaffold** klikněte na položku **seznamu**, pak klikněte na tlačítko **přidat**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Po kliknutí **přidat** tlačítko, *Views\Movies\SearchIndex.cshtml* vytvoření zobrazení šablony. Protože jste vybrali **seznamu** v **šablona Scaffold** seznamu, Visual Studio automaticky generovány (automaticky) některé výchozí značky v zobrazení. Základní kostry aplikace vytvořit formuláře HTML. Je zkontrolován `Movie` třídy a vytvořený kód pro vykreslení `<label>` prvky pro každou vlastnost třídy. Seznam níže zobrazuje zobrazení pro vytváření, která byla vygenerována:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Spusťte aplikaci a přejděte do */filmy/SearchIndex*. Připojte řetězec dotazu jako `?searchString=ghost` na adresu URL. Zobrazují se filtrované filmy.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Pokud se změní podpis `SearchIndex` metoda může mít parametr s názvem `id`, `id` parametr budou odpovídat `{id}` zástupný symbol pro výchozí trasy sady v *Global.asax* souboru.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

Původní `SearchIndex` metoda vypadá takto:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

Upravené `SearchIndex` metoda vypadá takto:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Nyní lze předat název vyhledávání jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Ale nemůžete očekávat, že uživatelům změnit adresu URL pokaždé, když chtějí hledat videa. Takže teď jste přidáte uživatelské rozhraní umožňující je filtrovat videa. Pokud jste změnili podpis `SearchIndex` metody testování jak předat parametru ID vázané na trasy, změňte ho tak, aby vaše `SearchIndex` metoda použije parametr řetězce s názvem `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Otevřít *Views\Movies\SearchIndex.cshtml* souboru a jenom po `@Html.ActionLink("Create New", "Create")`, přidejte následující:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

Následující příklad ukazuje část *Views\Movies\SearchIndex.cshtml* soubor se přidal filtrování značek.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm` Pomocné rutiny vytvoří počáteční `<form>` značky. `Html.BeginForm` Pomocné rutiny způsobí, že formulář ke zveřejnění na sebe sama, jakmile uživatel formulář odešle kliknutím **filtr** tlačítko.

Spusťte aplikaci a zkuste najít videa.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

Neexistuje žádná `HttpPost` přetížení `SearchIndex` metody. Není nutné, protože metoda není změněn stav aplikace, stačí filtrování dat.

Můžete přidat následující `HttpPost SearchIndex` metody. V takovém případě by odpovídala původce volání akce `HttpPost SearchIndex` metody a `HttpPost SearchIndex` spustili byste metodu, jak je znázorněno na následujícím obrázku.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Ale i v případě, že přidáte to `HttpPost` verzi `SearchIndex` metoda, existuje omezení v tom, jak to vše implementován. Představte si, že chcete konkrétní hledání (záložky) nebo chcete poslat odkaz s přáteli, mohou kliknout, chcete-li zobrazit stejné filtrovaný seznam videa. Všimněte si, že adresa URL pro odeslání požadavku HTTP POST je stejný jako adresu URL pro požadavek na získání (localhost:xxxxx/filmy/SearchIndex) – není k dispozici žádné informace o vyhledávání v adrese URL samotného. Pravé teď vyhledávací řetězec informace jsou odeslány na server jako hodnotu pole formuláře. To znamená, že nelze zachytit tyto informace vyhledávání (záložky) nebo odeslat přátel v adrese URL.

Řešením je použití přetížení `BeginForm` , která určuje, že požadavek POST by měla přidat informace o hledání na adresu URL a, že by měl směrovat na verzi třídy MetadataExchangeClientMode `SearchIndex` metody. Nahraďte existující konstruktor bez parametrů `BeginForm` metoda následujícím kódem:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Nyní když odešlete vyhledávání, adresa URL obsahuje hledaný řetězec dotazu. Hledání budou také moct `HttpGet SearchIndex` metodě akce, i když máte `HttpPost SearchIndex` metody.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Přidání vyhledávání podle žánru

Pokud jste přidali `HttpPost` verzi `SearchIndex` metoda, odstraňte ji.

V dalším kroku přidáte funkci, která umožňují uživatelům vyhledat filmy podle žánru. Nahradit `SearchIndex` metodu s následujícím kódem:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Tato verze `SearchIndex` metoda přijímá další parametr, a to `movieGenre`. Vytvoření první několika řádků kódu `List` objekt pro uložení žánry video z databáze.

Následující kód je dotaz LINQ, který načte všechny žánry z databáze.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

Tento kód použije `AddRange` metoda Obecné `List` kolekce pro přidání různých žánry do seznamu. (Bez `Distinct` modifikátor, byly přidány duplicitní žánry – například byly přidány komedie dvakrát v naší ukázce). Pak uloží seznam žánry v kódu `ViewBag` objektu.

Následující kód ukazuje, jak zkontrolovat `movieGenre` parametru. Pokud není prázdná, omezí kód dál filmy dotaz omezit vybrané videa k zadaným rozšířením podle tematických.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Přidání značek do zobrazení SearchIndex pro podporu vyhledávání podle žánru

Přidat `Html.DropDownList` pomocná rutina pro *Views\Movies\SearchIndex.cshtml* souboru, těsně před `TextBox` pomocné rutiny. Dokončený kód je zobrazena níže:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Spusťte aplikaci a přejděte do */filmy/SearchIndex*. Zkuste hledat podle žánru, název filmu a obě kritéria.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

V této části můžete prozkoumat metody akcí CRUD a vzhled zobrazení vygenerovaných sadou rozhraní framework. Můžete vytvořit metody akce vyhledávání a zobrazení, které umožňují uživatelům vyhledat tak, že název filmu a rozšířením podle tematických. V další části, budete pohledu na tom, jak přidat vlastnost, která má `Movie` modelu a přidání inicializátoru, který se automaticky vytvoří testovací databáze.

> [!div class="step-by-step"]
> [Předchozí](accessing-your-models-data-from-a-controller.md)
> [další](adding-a-new-field-to-the-movie-model-and-table.md)
