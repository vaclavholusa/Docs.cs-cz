---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Zkoumání metod Edit a zobrazení pro úpravy (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 4c0478f077ef22a301d21a5a8ce65d4117ee2348
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815259"
---
<a name="examining-the-edit-methods-and-edit-view-vb"></a>Zkoumání metod Edit a zobrazení pro úpravy (VB)
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
> Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu. [Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/examining-the-edit-methods-and-edit-view.md) tohoto kurzu.


V této části budete prozkoumat metody generované akce a zobrazení kontroleru video. Potom přidáte vlastní stránku vyhledávání.

Spusťte aplikaci a přejděte `Movies` řadič přidáním */Movies* na adresu URL do adresního řádku prohlížeče. Podržte ukazatel myši nad **upravit** na adresu URL, která odkazuje na odkaz.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Upravit** byl vytvořen odkaz `Html.ActionLink` metodu *Views\Movies\Index.vbhtml* zobrazení:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Pomocné rutiny, která je vystavena pomocí vlastnosti ve je objekt `WebViewPage` základní třídy. `ActionLink` Metody pomocné rutiny usnadňuje dynamicky generovat hypertextové odkazy HTML, které jsou propojeny do metody akce v řadičích. První argument `ActionLink` metoda je text odkazu pro vykreslení (například `<a>Edit Me</a>`). Druhým argumentem je název metody akce, která se má vyvolat. Konečný argument je [anonymní objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) , který generuje data trasy (v tomto případě ID 4).

Vygenerovaný odkaz je vidět na předchozím obrázku je `http://localhost:xxxxx/Movies/Edit/4`. Vzor adresy URL trvá výchozí trasu `{controller}/{action}/{id}`. Proto převede ASP.NET `http://localhost:xxxxx/Movies/Edit/4` do požadavku na `Edit` metody akce `Movies` řadiče s parametrem `ID` rovna 4.

Můžete také předat parametry metody akce pomocí řetězce dotazu. Například adresa URL `http://localhost:xxxxx/Movies/Edit?ID=4` také předá parametr `ID` 4 `Edit` metody akce `Movies` kontroleru.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Otevřít `Movies` kontroleru. Dva `Edit` níže jsou uvedené metody akce.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Všimněte si, že druhá `Edit` předchází metody akce `HttpPost` atribut. Tento atribut určuje, že přetížení `Edit` metodu lze vyvolat pouze pro požadavky POST. Můžete použít `HttpGet` atribut na první upravit metodu, ale to není nezbytné vzhledem k tomu, že je výchozí hodnota. (Budeme odkazovat na metody akce, které jsou přiřazeny implicitně `HttpGet` atribut jako `HttpGet` metody.)

`HttpGet` `Edit` Metoda přijímá parametr ID film, vyhledá videa pomocí Entity Frameworku `Find` metoda a vrátí vybraného videa do zobrazení pro úpravy. Když systém generování uživatelského rozhraní zobrazení pro úpravy, prozkoumat `Movie` třídy a vytvořený kód pro vykreslení `<label>` a `<input>` prvky pro každou vlastnost třídy. Následující příklad ukazuje zobrazení pro úpravy, která byla vygenerována:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Všimněte si, jak se má zobrazit šablonu `@ModelType MvcMovie.Models.Movie` příkazu v horní části souboru, toto nastavení určuje, že zobrazení očekává, že model pro zobrazení šablony, která má být typu `Movie`.

Automaticky generovaný kód používá několik *pomocné metody* zjednodušit značka jazyka HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Název pole, zobrazí pomocná (&quot;Title&quot;, &quot;ReleaseDate&quot;, &quot;žánr&quot;, nebo &quot;cena &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) HTML zobrazí pomocná `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocné rutiny zobrazí všechny zprávy ověření přidružené k této vlastnosti.

Spusťte aplikaci a přejděte */Movies* adresy URL. Klikněte na tlačítko **upravit** odkaz. V prohlížeči zobrazte zdroj stránky. HTML na stránce vypadá jako v následujícím příkladu. (Značky nabídky byl vyloučen pro přehlednost).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

`<input>` Prvky jsou v kódu HTML `<form>` elementu jehož `action` atribut je nastaven na Odeslat požadavek POST */filmy/úpravy* adresy URL. Publikuje se data formuláře na serveru při **upravit** po kliknutí na tlačítko.

## <a name="processing-the-post-request"></a>Zpracování požadavku POST

Následující seznam ukazuje `HttpPost` verzi `Edit` metody akce.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

Vazač modelu ASP.NET framework vezme hodnoty odeslaného formuláře a vytvoří `Movie` objektu, který je předán jako `movie` parametru. `ModelState.IsValid` Vrácení se změnami kód ověří, že odeslaná data do formuláře je možné upravit `Movie` objektu. Pokud jsou data platná, uloží kód pro data o filmech `Movies` kolekce `MovieDBContext` instance. Kód pak uloží novou data o filmech k databázi pomocí volání `SaveChanges` metoda `MovieDBContext`, které ukládají změny v databázi. Po uložení dat, kód přesměruje uživatele `Index` metody akce `MoviesController` třídy, což způsobí, že aktualizovaný video zobrazí v seznamu videa.

Pokud odeslaných hodnot nejsou platné, se zobrazí znovu ve formuláři. `Html.ValidationMessageFor` Pomocné rutiny v *Edit.vbhtml* zobrazení šablony postará o zobrazení odpovídající chybové zprávy.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Poznámka o národních prostředích** Pokud pracujete s národním prostředí než v angličtině, přečtěte si téma [podpůrné technologie ASP.NET MVC 3 ověření pomocí jiné než anglické národní prostředí.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Provádění více robustní úpravy – metoda

`HttpGet` `Edit` Metoda systém generování uživatelského rozhraní vygeneruje neprovádí kontrolu, že je platný Identifikátor, který je předán. Pokud uživatel zruší adresa URL ID segmentu (`http://localhost:xxxxx/Movies/Edit`), zobrazí se následující chyba:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Uživatel může také předat ID, která neexistuje v databázi, jako například `http://localhost:xxxxx/Movies/Edit/1234`. Můžete vytvořit dvě změny `HttpGet` `Edit` metody akce k vyřešení tohoto omezení. Nejprve změňte `ID` parametr mít výchozí hodnotu nula, pokud není explicitně předané ID. Můžete také zkontrolovat, který `Find` skutečně nalezena metoda filmu před vrácením video k šabloně zobrazení. Aktualizovaný `Edit` metoda je uveden níže.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Pokud se nenajde žádný film, `HttpNotFound` metoda je volána.

Všechny `HttpGet` metody podobné tvar. Dostanou video (nebo seznam objektů, v případě `Index`) a předána do zobrazení modelu. `Create` Metoda předává objekt prázdný film zobrazení pro vytváření. Všechny metody, které vytvořit, upravit, odstranit nebo jinak upravit data, proveďte `HttpPost` přetížení metody. Úpravy dat v metodě GET protokolu HTTP je bezpečnostním rizikem, jak je popsáno v příspěvku blogu [ASP.NET MVC Tip #46 – nepoužívejte odstranit odkazy, protože uživatel vytvořit bezpečnostní díry](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Úpravy dat v metodě GET také porušuje HTTP osvědčené postupy a architektury REST vzor, který určuje, že požadavky GET by neměly měnit stav vaší aplikace. Jinými slovy provádění operace GET by měl být bezpečný provoz, který nemá žádné vedlejší účinky.

## <a name="adding-a-search-method-and-search-view"></a>Přidání vyhledávací metody a zobrazení vyhledávání

V této části přidáte `SearchIndex` metody akce, která umožňuje hledat filmy podle žánru nebo názvu. To bude k dispozici pomocí */filmy/SearchIndex* adresy URL. Žádost se zobrazí formulář HTML, který obsahuje vstupní prvky, které uživatel můžete přejít k vyplnění aby bylo možné hledat videa. Jakmile uživatel formulář odešle, metoda akce získat hodnoty vyhledávání publikovaných uživatelem, který se hodnoty slouží k vyhledání databáze.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Zobrazení formuláře SearchIndex

Začněte přidáním `SearchIndex` metody akce ke stávající `MoviesController` třídy. Metoda vrátí zobrazení obsahující formulář HTML. Zde je kód:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

První řádek `SearchIndex` metoda vytvoří následující [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) dotaz pro výběr videa:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

Dotaz je definován v tomto okamžiku, ale nebyl dosud spuštěn proti úložišti.

Pokud `searchString` parametr obsahuje řetězec, dotaz filmy je upravit tak, aby filtrování na základě hodnoty hledaný řetězec, pomocí následujícího kódu:

Pokud není String.IsNullOrEmpty(searchString) pak   
 filmy = videa. Kde (funkce s.Title.Contains(searchString))   
 End If

Dotazy LINQ nejsou provedeny, když jsou definovány, nebo při jejich změně voláním metody `Where` nebo `OrderBy`. Místo toho provádění dotazu je odloženo, což znamená, že vyhodnocení výrazu je odloženo jeho očekávané hodnoty ve skutečnosti procházen nebo [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metoda je volána. V `SearchIndex` ukázkové spuštění dotazu v zobrazení SearchIndex. Další informace o odložený dotaz, naleznete v tématu [provádění dotazu](https://msdn.microsoft.com/library/bb738633.aspx).

Teď můžete implementovat `SearchIndex` zobrazení, které se uživateli zobrazí formulář. Klepněte pravým tlačítkem myši `SearchIndex` metody a pak klikněte na tlačítko **přidat zobrazení**. V **přidat zobrazení** dialogového okna zadejte, že budete předávat `Movie` objektu, který chcete zobrazit šablonu jako jeho třída modelu. V **šablona Scaffold** klikněte na položku **seznamu**, pak klikněte na tlačítko **přidat**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Po kliknutí **přidat** tlačítko, *Views\Movies\SearchIndex.vbhtml* vytvoření zobrazení šablony. Protože jste vybrali **seznamu** v **šablona Scaffold** seznamu, automaticky vygenerovat aplikaci Visual Web Developer (automaticky) některé výchozí obsah v zobrazení. Základní kostry aplikace vytvořit formuláře HTML. Je zkontrolován `Movie` třídy a vytvořený kód pro vykreslení `<label>` prvky pro každou vlastnost třídy. Seznam níže zobrazuje zobrazení pro vytváření, která byla vygenerována:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Spusťte aplikaci a přejděte do */filmy/SearchIndex*. Připojte řetězec dotazu jako `?searchString=ghost` na adresu URL. Zobrazují se filtrované filmy.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Pokud se změní podpis `SearchIndex` metoda může mít parametr s názvem `id`, `id` parametr budou odpovídat `{id}` zástupný symbol pro výchozí trasy sady v *Global.asax* souboru.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Upravené `SearchIndex` metoda vypadá takto:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Nyní lze předat název vyhledávání jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Ale nemůžete očekávat, že uživatelům změnit adresu URL pokaždé, když chtějí hledat videa. Takže teď jste přidáte uživatelské rozhraní umožňující je filtrovat videa. Pokud jste změnili podpis `SearchIndex` metody testování jak předat parametru ID vázané na trasy, změňte ho tak, aby vaše `SearchIndex` metoda použije parametr řetězce s názvem `searchString`:

Otevřít *Views\Movies\SearchIndex.vbhtml* souboru a jenom po `@Html.ActionLink("Create New", "Create")`, přidejte následující:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

`Html.BeginForm` Pomocné rutiny vytvoří počáteční `<form>` značky. `Html.BeginForm` Pomocné rutiny způsobí, že formulář ke zveřejnění na sebe sama, jakmile uživatel formulář odešle kliknutím **filtr** tlačítko.

Spusťte aplikaci a zkuste najít videa.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

Neexistuje žádná `HttpPost` přetížení `SearchIndex` metody. Není nutné, protože metoda není změněn stav aplikace, stačí filtrování dat. Pokud jste přidali následující `HttpPost` `SearchIndex` metody původce volání akce odpovídají `HttpPost` `SearchIndex` metody a `HttpPost` `SearchIndex` spustili byste metodu, jak je znázorněno na následujícím obrázku.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Přidání vyhledávání podle žánru

Pokud jste přidali `HttpPost` verzi `SearchIndex` metoda, odstraňte ji.

V dalším kroku přidáte funkci, která umožňují uživatelům vyhledat filmy podle žánru. Nahradit `SearchIndex` metodu s následujícím kódem:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Tato verze `SearchIndex` metoda přijímá další parametr, a to `movieGenre`. Vytvoření první několika řádků kódu `List` objekt pro uložení žánry video z databáze.

Následující kód je dotaz LINQ, který načte všechny žánry z databáze.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

Tento kód použije `AddRange` metoda Obecné `List` kolekce pro přidání různých žánry do seznamu. (Bez `Distinct` modifikátor, byly přidány duplicitní žánry – například byly přidány komedie dvakrát v naší ukázce). Pak uloží seznam žánry v kódu `ViewBag` objektu.

Následující kód ukazuje, jak zkontrolovat `movieGenre` parametru. Pokud není prázdný kód dál omezuje dotaz filmy omezit vybrané videa k zadaným rozšířením podle tematických.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Přidání značek do zobrazení SearchIndex pro podporu vyhledávání podle žánru

Přidat `Html.DropDownList` pomocná rutina pro *Views\Movies\SearchIndex.vbhtml* souboru, těsně před `TextBox` pomocné rutiny. Dokončený kód je zobrazena níže:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Spusťte aplikaci a přejděte do */filmy/SearchIndex*. Zkuste hledat podle žánru, název filmu a obě kritéria.

V této části můžete prozkoumat metody akcí CRUD a vzhled zobrazení vygenerovaných sadou rozhraní framework. Můžete vytvořit metody akce vyhledávání a zobrazení, které umožňují uživatelům vyhledat tak, že název filmu a rozšířením podle tematických. V další části, budete pohledu na tom, jak přidat vlastnost, která má `Movie` modelu a přidání inicializátoru, který se automaticky vytvoří testovací databáze.

> [!div class="step-by-step"]
> [Předchozí](accessing-your-models-data-from-a-controller.md)
> [další](adding-a-new-field.md)
