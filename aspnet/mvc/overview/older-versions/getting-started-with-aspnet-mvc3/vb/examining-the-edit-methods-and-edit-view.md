---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Zkoumání upravit metody a zobrazení (VB) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: ab55de16baea3010f95c8f23a093544fb1fd7aa3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Zkoumání upravit metody a zobrazení (VB)
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se VB.NET zdrojový kód je k dispozici v tomto tématu. [Stáhnout verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přepnout [C# verze](../cs/examining-the-edit-methods-and-edit-view.md) tohoto kurzu.


V této části budete zkontrolujte metody generované akce a zobrazení pro film kontroleru. Potom přidáte vlastní vyhledávací stránku.

Spusťte aplikaci a přejděte do `Movies` řadiče připojením */Movies* na adresu URL na panelu Adresa prohlížeče. Podržte ukazatel myši nad **upravit** na adresu URL, která obsahuje odkazy na odkaz.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Upravit** odkaz vygenerovalo `Html.ActionLink` metoda v *Views\Movies\Index.vbhtml* zobrazení:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Objekt je pomocné rutiny, který je zveřejněný pomocí vlastnosti na `WebViewPage` základní třídy. `ActionLink` Metoda pomocníka, který usnadňuje dynamicky generovat hypertextové odkazy HTML, které odkazují na metody akce na řadiče. První argument `ActionLink` metoda je text odkazu k vykreslení (například `<a>Edit Me</a>`). Druhý argument je název metody akce, která bude vyvolána. Konečný argument je [anonymní objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) který generuje data trasy (v tomto případě ID 4).

Vygenerovaný odkaz viz předchozí obrázek je `http://localhost:xxxxx/Movies/Edit/4`. Výchozí trasu trvá vzor adresy URL `{controller}/{action}/{id}`. Proto ASP.NET překládá `http://localhost:xxxxx/Movies/Edit/4` do požadavek na `Edit` metody akce `Movies` řadiče s parametrem `ID` rovna 4.

Také můžete předat parametry metody akce pomocí řetězce dotazu. Například adresa URL `http://localhost:xxxxx/Movies/Edit?ID=4` také předá parametr `ID` 4 k `Edit` metody akce `Movies` řadiče.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Otevřete `Movies` řadiče. Dva `Edit` metody akce, které jsou uvedeny níže.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Všimněte si, druhý `Edit` předchází metody akce `HttpPost` atribut. Tento atribut určuje, že přetížení `Edit` metoda může být volána pouze pro požadavky POST. Je možné aplikovat `HttpGet` atribut prvního upravit metodu, ale který není nutné protože je výchozí hodnota. (Budeme označovat metody akce, které jsou implicitně přiřazené `HttpGet` atribut jako `HttpGet` metody.)

`HttpGet` `Edit` Metoda přebírá parametr ID film, vyhledává film používající rozhraní Entity Framework `Find` metoda a vrátí vybraného videa na zobrazení pro úpravy. Při generování uživatelského rozhraní systému vytvoření zobrazení pro úpravy, zkontrolován `Movie` třídy a vytvořený kód k vykreslení `<label>` a `<input>` prvky pro každou vlastnost třídy. Následující příklad ukazuje zobrazení upravit, která byla vygenerována:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Všimněte si, jak se má zobrazit šablonu `@ModelType MvcMovie.Models.Movie` příkaz v horní části souboru – tato hodnota určuje, že zobrazení očekává, že model pro zobrazení šablony být typu `Movie`.

Automaticky generovaný kód používá několik *pomocné metody* zefektivnění kód HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocné rutiny zobrazuje název pole (&quot;název&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, nebo &quot;cena &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Zobrazí pomocné rutiny HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocník zobrazí všechny zprávy ověření přidružené k této vlastnosti.

Spusťte aplikaci a přejděte do */Movies* adresy URL. Klikněte na tlačítko **upravit** odkaz. V prohlížeči zobrazte zdroj pro stránku. Na stránce HTML vypadá jako v následujícím příkladu. (Kód nabídky se vyloučila pro přehlednost).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

`<input>` Prvky jsou v kódu HTML `<form>` element jejichž `action` k vystavování příspěvků na nastavený atribut *nebo filmy nebo upravit* adresy URL. Data formuláře budou odeslány na server při **upravit** po kliknutí na tlačítko.

## <a name="processing-the-post-request"></a>Zpracování požadavku POST

Následující seznam uvádí `HttpPost` verzi `Edit` metody akce.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

Vazač modelu ASP.NET framework přijímá hodnoty odeslaného formuláře a vytvoří `Movie` objekt, který se předá jako `movie` parametr. `ModelState.IsValid` Vrácení se změnami kódu ověřuje, že odeslaná data do formuláře lze použít k úpravě `Movie` objektu. Pokud jsou data platná, uloží kód film data, která mají `Movies` kolekce `MovieDBContext` instance. Kód pak uloží novou film data do databáze při volání `SaveChanges` metodu `MovieDBContext`, které ukládají změny databáze. Po uložení dat, kód přesměruje uživatele `Index` metody akce `MoviesController` třídy, což způsobí, že aktualizované film, který se má zobrazit ve výpisu filmy.

Pokud odeslaných hodnot nejsou platné, se zobrazí znovu ve formuláři. `Html.ValidationMessageFor` Pomocné rutiny v *Edit.vbhtml* zobrazení šablony postará o zobrazení příslušné chybové zprávy.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Poznámka o národních prostředích** Pokud pracujete s národní prostředí než angličtiny, přečtěte si téma [podpora ASP.NET MVC 3 ověřování pomocí jiné než anglické národní prostředí.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Provedení větší odolnost metoda úprav

`HttpGet` `Edit` Metoda generované systémem generování uživatelského rozhraní neohlásí, že je Identifikátor, který je předán platný. Pokud uživatel odebere ID segmentu z adresy URL (`http://localhost:xxxxx/Movies/Edit`), zobrazí se chybová zpráva:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Uživatel může předat také ID, které neexistuje v databázi, jako například `http://localhost:xxxxx/Movies/Edit/1234`. Můžete provádět dva změny na `HttpGet` `Edit` metody akce k vyřešení toto omezení. Nejprve změnit `ID` do mají výchozí hodnotu nula, pokud ID není předán explicitně parametr. Můžete také zkontrolovat, který `Find` ve skutečnosti nalezena metoda film před vrácením video zobrazit šablonu. Aktualizovaný `Edit` metoda jsou uvedeny níže.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Pokud se nenajde žádný film, `HttpNotFound` metoda je volána.

Všechny `HttpGet` podobný Princip podle metody. Získají objekt film (nebo seznam objektů, v případě `Index`) a předat zobrazení modelu. `Create` Metoda předá objekt prázdný film zobrazení pro vytváření. Takže v udělat všechny metody, které vytvořit, upravit, odstranit nebo jinak měnit data `HttpPost` přetížení metody. Úprava dat v metodu GET protokolu HTTP je bezpečnostním rizikem, jak je popsáno v příspěvku blogu [ASP.NET MVC Tip #46 – nepoužívejte odkazy odstranit, protože uživatel vytvořit celistvosti](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Úprava dat v metodu GET také porušuje HTTP osvědčené postupy a architektury vzorec REST, která určuje, že požadavky GET nesmí změnit stav vaší aplikace. Jinými slovy při provádění operace GET by měl být bezpečný provoz, který má žádné vedlejší účinky.

## <a name="adding-a-search-method-and-search-view"></a>Přidání metodu Search a zobrazení vyhledávání

V této části přidáte `SearchIndex` metody akce, která umožňuje vyhledávání filmy genre nebo název. To bude k dispozici pomocí */filmy/SearchIndex* adresy URL. Žádost se zobrazí formuláře HTML, který obsahuje vstupní prvky, které chcete-li vyhledat film můžete vyplnit uživatele. Když uživatel formulář odešle, metoda akce se získat hodnoty vyhledávání vystavil uživatele a použití hodnot pro vyhledávání databázi.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Zobrazení SearchIndex formuláře

Začněte přidáním `SearchIndex` metoda akce ke stávající `MoviesController` třídy. Metoda vrátí zobrazení, která obsahuje formuláře HTML. Zde je kód:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

První řádek `SearchIndex` metoda vytvoří následující [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotazu a vyberte filmy:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

Dotaz je definována v tomto okamžiku, ale dosud nebyla spuštěna před úložišti.

Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na základě hodnoty řetězec pro hledání, pomocí následujícího kódu:

Pokud není String.IsNullOrEmpty(searchString) pak   
 filmy = filmy. Kde (byly funkce s.Title.Contains(searchString))   
 Ukončení, pokud

Dotazy LINQ nebudou provedeny, když jsou definovány nebo když jsou upraveny voláním metody `Where` nebo `OrderBy`. Místo toho při provádění dotazu je odložení, což znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je ve skutečnosti vstupní přes nebo [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda je volána. V `SearchIndex` ukázku, je dotaz proveden v SearchIndex zobrazení. Další informace o provádění odložené dotazů najdete v tématu [provádění dotazu](https://msdn.microsoft.com/library/bb738633.aspx).

Teď můžete implementovat `SearchIndex` zobrazení, které se uživateli zobrazí formulář. Klepněte pravým tlačítkem myši `SearchIndex` metoda a pak klikněte na tlačítko **přidat zobrazení**. V **přidat zobrazení** dialogovém okně zadejte, že budete předávat `Movie` objekt, který chcete zobrazit šablonu jako jeho třídu modelu. V **vygenerované uživatelské rozhraní šablony** vyberte **seznamu**, pak klikněte na tlačítko **přidat**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Když kliknete **přidat** tlačítko *Views\Movies\SearchIndex.vbhtml* vytvoření šablony zobrazení. Vzhledem k tomu, že jste vybrali **seznamu** v **vygenerované uživatelské rozhraní šablony** seznamu, Visual Web Developer automaticky generovány (vygeneroval) některé výchozí obsah v zobrazení. Generování uživatelského rozhraní vytvořit formuláře HTML. Je zkontrolován `Movie` třídy a vytvořený kód k vykreslení `<label>` prvky pro každou vlastnost třídy. Seznam dole ukazuje vytvořit zobrazení, která byla vygenerována:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Spusťte aplikaci a přejděte do */filmy/SearchIndex*. Připojit řetězec dotazu, jako `?searchString=ghost` na adresu URL. Filtrované filmy jsou zobrazeny.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Pokud změníte podpis `SearchIndex` metoda tak, aby měl parametr s názvem `id`, `id` parametr bude shodovat s `{id}` zástupný symbol pro výchozí směruje sada v *Global.asax* souboru.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Upravenou `SearchIndex` metoda vypadat takto:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Název hledání můžete nyní předat jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Nelze však budou uživatelé chcete upravit adresu URL pokaždé, když chtějí hledat film. Ano, teď můžete přidáte uživatelského rozhraní, můžete je filtrovat filmy. Pokud jste změnili podpis `SearchIndex` metoda k testování jak předat parametr ID vázané na trasy ho změnit tak, aby vaše `SearchIndex` metoda přijímá řetězcový parametr s názvem `searchString`:

Otevřete *Views\Movies\SearchIndex.vbhtml* souborů a právě po `@Html.ActionLink("Create New", "Create")`, přidejte následující:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

`Html.BeginForm` Pomocné rutiny vytvoří počáteční `<form>` značky. `Html.BeginForm` Pomocná způsobí, že formulář post sám na sebe, když uživatel odešle formulář kliknutím **filtru** tlačítko.

Spusťte aplikaci a zkuste vyhledat film.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Neexistuje žádné `HttpPost` přetížení z `SearchIndex` metoda. Není nutné, protože metoda není změny stavu aplikace, právě filtrování dat. Pokud jste přidali následující `HttpPost` `SearchIndex` metody původce volání akce odpovídá `HttpPost` `SearchIndex` metody a `HttpPost` `SearchIndex` metoda by spustit jak je znázorněno na obrázku níže.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Přidání vyhledávání podle Genre

Pokud jste přidali `HttpPost` verzi `SearchIndex` metoda, odstraňte jej.

V dalším kroku přidáte funkci tak, aby uživatelé vyhledejte filmy podle genre. Nahraďte `SearchIndex` metoda následujícím kódem:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Tato verze `SearchIndex` metoda přebírá další parametr, a to `movieGenre`. Vytvoření první několika řádků kódu `List` objekt pro uložení žánry film z databáze.

Následující kód je dotaz LINQ, který načte všechny žánry z databáze.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

Kód používá `AddRange` metoda obecná `List` kolekce odlišné žánry přidat do seznamu. (Bez `Distinct` modifikátor, by byl přidán duplicitní žánry – například komedie by byl přidán dvakrát v naše ukázka). Kód pak uloží seznam žánry v `ViewBag` objektu.

Následující kód ukazuje, jak zkontrolovat `movieGenre` parametr. Pokud není prázdná omezí kód další dotaz filmy omezit vybrané filmy k zadané genre.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Přidání značek k zobrazení SearchIndex pro podporu vyhledávání podle Genre

Přidat `Html.DropDownList` pomocníka, který má *Views\Movies\SearchIndex.vbhtml* souboru, těsně před `TextBox` pomocné rutiny. Dokončený kód je zobrazena níže:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Spusťte aplikaci a přejděte do */filmy/SearchIndex*. Zkuste hledání genre, název film a obě kritéria.

V této části posouzení metody akce CRUD a zobrazení generované rozhraní. Můžete vytvořit metody akce vyhledávání a zobrazení, která umožní uživatelům vyhledávat podle názvu film a genre. V další části se budete podíváte na tom, jak přidat vlastnost, která má `Movie` modelu a postup přidání inicializátoru, který se automaticky vytvoří testovací databáze.

> [!div class="step-by-step"]
> [Předchozí](accessing-your-models-data-from-a-controller.md)
> [další](adding-a-new-field.md)
